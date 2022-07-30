<script lang="ts">
	import IssueCard from './components/IssueCard.svelte'
	import type { Issue } from './interfaces/Issue'
	import { onMount } from 'svelte'
	import Modal from './components/IssueModal.svelte'

	let isOpen = false
	let issues: Issue[] = []

	onMount(async () => {
		getIssues()
	})

	const getIssues = async () => {
		const res = await fetch('http://localhost:3000/issues')
		issues = await res.json()
	}

	const deleteIssue = (id: number) => {
		issues = issues.filter(issue => issue.id !== id)
		fetch(`http://localhost:3000/issues/${id}`, {
			method: 'DELETE',
		})
	}

	const addIssue = (issue: Issue) => {
		fetch('http://localhost:3000/issues', {
			method: 'POST',
			headers: {
				'content-type': 'application/json',
			},
			body: JSON.stringify(issue),
		}).finally(() => {
			getIssues()
			isOpen = false
		})
	}
</script>

<svelte:head>
	<link
		rel="stylesheet"
		href="https://cdnjs.cloudflare.com/ajax/libs/bootswatch/5.1.3/lux/bootstrap.min.css"
		integrity="sha512-B5sIrmt97CGoPUHgazLWO0fKVVbtXgGIOayWsbp9Z5aq4DJVATpOftE/sTTL27cu+QOqpI/jpt6tldZ4SwFDZw=="
		crossorigin="anonymous"
		referrerpolicy="no-referrer"
	/>
	<script
		src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
		integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
		crossorigin="anonymous"></script>
</svelte:head>

<div class="border-bottom py-4 text-center text-uppercase fs-4">Issues Manager</div>
<div class="container my-4">
	<button class="btn btn-primary mb-4" on:click={() => (isOpen = true)}>New Issue</button>
	<div class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4">
		{#each issues as issue}
			<IssueCard {issue} {deleteIssue} />
		{/each}
	</div>
</div>
<Modal bind:isOpen {addIssue} />
