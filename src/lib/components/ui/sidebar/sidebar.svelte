<script lang="ts">
	import { Folder, Plus, Settings, PanelRightOpen } from 'lucide-svelte';
	import { Button } from '$lib/components/ui/button';
	import { Input } from '$lib/components/ui/input';
	import { createActiveResearch, createResearchList } from '@/states.svelte';
	import { tick } from 'svelte';
    import { goto } from '$app/navigation';

	const activeResearch = createActiveResearch();
	const researchList = createResearchList();

	let props = $props();
	let isAddNewResearch: boolean = $state(false);
	let newResearchName: string = $state('');
	let researchNameInputComponent: HTMLInputElement | null = $state(null);

	researchList.init(props.research);

    const changeRoute = () => {
        activeResearch.update(researchList.latest())
        goto(`/dashboard/${researchList.latest()}`)
    }
</script>

<div class="flex max-w-64 min-w-64 flex-col bg-white h-full">
	<div class="p-4 flex justify-between items-center">
		<h1 class="text-xl font-bold text-blue-600">HiveCite</h1>
		<PanelRightOpen size={18} class="cursor-pointer text-gray-400"/>
	</div>
	<div class="p-4">
		<Button
			class="w-full"
			variant="theme" 
			on:click={async () => {
				isAddNewResearch = true;
				await tick();
				if (researchNameInputComponent) {
					researchNameInputComponent.focus();
				}
			}}
		>
			<Plus />
			<span>New Research</span>
		</Button>
	</div>
	<div class="px-4 py-2 h-full overflow-auto">
		<h2 class="mb-2 text-sm font-semibold text-gray-500">Research</h2>
		<ul>
			{#each researchList.list as research}
				<a href="/dashboard/{research}">
					<li
						class="mb-1 rounded-md {research === activeResearch.value
							? 'bg-blue-50 text-blue-600'
							: ''}"
					>
						<button
							class="flex w-full cursor-pointer items-center px-3 py-2 text-left hover:rounded-md hover:bg-blue-50 hover:text-blue-600"
							onclick={() => activeResearch.update(research)}
						>
							<Folder class="mr-2" />
							<span class="truncate"
								>{research.length > 20 ? research.substring(0, 14) + '...' : research}</span
							>
						</button>
					</li>
				</a>
			{/each}
			{#if researchList.list.length === 0 && isAddNewResearch === false}
				<p class="text-sm italic">No research</p>
			{/if}
			{#if isAddNewResearch}
				<li class="mb-1 rounded-md">
					<button
						class="flex w-full cursor-pointer items-center px-3 py-2 text-left hover:rounded-md hover:bg-blue-50"
					>
						<Folder class="mr-2" />
						<Input
							bind:value={newResearchName}
							bind:ref={researchNameInputComponent!}
							onfocusout={() => {
								isAddNewResearch = false;
								newResearchName = '';
							}}
							onkeydown={(e) => {
								if (e.key === 'Enter' && newResearchName !== '') {
									researchList.add(newResearchName);
									newResearchName = '';
									isAddNewResearch = false;
                                    changeRoute()
								}
							}}
						/>
					</button>
				</li>
			{/if}
		</ul>
	</div>
	<a href="/setting" class="mt-auto border-t border-gray-200 p-4 cursor-pointer hover:bg-gray-200">
		<div class="flex items-center gap-3">
			<Settings />
			<p>Settings</p>
		</div>
	</a>
</div>
