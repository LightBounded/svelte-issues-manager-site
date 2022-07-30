<script lang="ts">
	import Modal from 'sv-bootstrap-modal'
	import type { Issue } from '../interfaces/Issue'
	export let isOpen: Boolean
	export let addIssue: Function

	const handleSubmit = (event: Event) => {
		const form: HTMLFormElement = event.target as HTMLFormElement
		
		form.classList.add('was-validated')
		if (!form.checkValidity()) return
		form.classList.remove('was-validated')

		const formData: Issue = Object.fromEntries(new FormData(form)) as unknown as Issue
		addIssue(formData)
	}
</script>

<Modal bind:open={isOpen}>
	<div class="modal-header">
		<h5 class="modal-title">Add an issue</h5>
		<button type="button" class="btn-close" on:click={() => (isOpen = false)} />
	</div>
	<div class="modal-body">
		<form id="issue-form" on:submit|preventDefault={handleSubmit}>
			<div class="mb-3">
				<label class="form-label" for="person">Name</label>
				<input class="form-control" name="person" id="person" type="text" required />
			</div>
			<div class="mb-3">
				<label class="form-label" for="title">Title</label>
				<input class="form-control" name="title" id="title" type="text" required />
			</div>
			<div class="mb-3">
				<label class="form-label" for="description">Description</label>
				<textarea class="form-control" name="description" id="description" required />
			</div>
			<div>
				<label class="form-label" for="severity">Severity</label>
				<select name="severity" id="severity" class="form-select" required>
					<option selected value="">Select a severity</option>
					<option value="high">High</option>
					<option value="medium">Medium</option>
					<option value="low">Low</option>
				</select>
			</div>
		</form>
	</div>
	<div class="modal-footer">
		<button type="button" class="btn btn-secondary" on:click={() => (isOpen = false)}
			>Close</button
		>
		<button form="issue-form" type="submit" class="btn btn-primary">Save changes</button>
	</div>
</Modal>
