<script lang="ts">
	let {
		changes,
		condition = undefined
	}: {
		changes: string[];
		condition?: string;
	} = $props();
</script>

<span>
	{#each changes as change}
		<code class="form">{change}</code>
	{/each}
</span>

{#if condition}
	<code class="condition">{condition}</code>
{/if}

<style>
	code.form:not(:last-child)::after {
		content: ' > ';
	}

	code.condition::before {
		content: ' / ';
	}
</style>
