<script lang="ts">
	import type { Issue } from '../interfaces/Issue'

	export let issue: Issue
	export let deleteIssue: Function

	const toggleIssue = () => {
		issue = { ...issue, isOpen: !issue.isOpen }
		fetch(`http://localhost:3000/issues/${issue.id}`, {
			method: 'PUT',
			headers: {
				'content-type': 'application/json',
			},
			body: JSON.stringify(issue),
		})
	}
</script>

<div class="col">
	<div class="card">
		<div class="card-header d-flex align-items-center justify-content-between">
			<div>Issued by {issue.person}</div>
			<div class="dropstart">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
					style="height:1.5em;width:1.5em;cursor:pointer"
					data-bs-toggle="dropdown"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z"
					/>
				</svg>
				<ul class="dropdown-menu">
					<li class="dropdown-item" on:click={toggleIssue}>
						{issue.isOpen ? 'Close' : 'Open'}
					</li>
					<li class="dropdown-item" on:click={deleteIssue(issue.id)}>Delete</li>
				</ul>
			</div>
		</div>
		<div class="card-body">
			<h4>{issue.title}</h4>
			<p>{issue.description}</p>
			<h4>Severity</h4>
			<p class="text-capitalize">{issue.severity}</p>
			<span
				class="badge rounded-pill text-capitalize 
				{issue.isOpen ? 'bg-success' : 'bg-danger'}"
			>
				{issue.isOpen ? 'Open' : 'Closed'}
			</span>
		</div>
	</div>
</div>
