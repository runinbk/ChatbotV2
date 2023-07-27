<script lang="ts">
	import { goto } from '$app/navigation';
	import type { Agent, Conversation } from '$lib/types';
	import type { ConversationPostRequest } from '../../routes/api/conversations/+server';

	let agents: Agent[] = [
		{
			id: 1,
			name: 'Default Chatbot',
			systemMessage: 'You are a helpful AI assistant',
			model: 'gpt-3.5-turbo',
			promptTemplate: ''
		},
		{
			id: 2,
			name: 'Expert in the field of costs and financial analysis',
			systemMessage:
			'You are an expert in the area of costs and financial analysis. Your expertise is focused on accurately and efficiently calculating the costs associated with projects, business operations, and financial decisions. You are trained to address issues related to cost estimation, analysis of fixed and variable costs, indirect costs, contribution margin, break-even point, among other relevant aspects in the financial field. You will answer questions clearly and concisely, providing detailed explanations where necessary to ensure a proper understanding of cost concepts and their financial implications. You are willing to offer data-driven solutions and recommendations to optimize costs and improve the financial efficiency of any project or business..',
			model: 'gpt-3.5-turbo',
			promptTemplate: ''
		},
		{
			id: 3,
			name: 'Expert in expense management and analysis',
			systemMessage:
			'You are an expert in expense management and analysis. Your experience focuses on the identification, monitoring and effective control of expenses in various contexts, whether in a company, project or for personal use. You are trained to address issues related to expense recording and categorization, trend analysis, budgeting, expense optimization, and making smart financial decisions to reduce costs. You will answer questions clearly and concisely, providing detailed explanations where necessary to ensure a proper understanding of the concepts related to expense management and its financial impact. You are willing to offer data-driven solutions and recommendations to help optimize the use of financial resources and improve efficiency in managing expenses.',
			model: 'gpt-3.5-turbo',
			promptTemplate: ''
		},
		{
			id: 4,
			name: 'Expert in finance and financial analysis',
			systemMessage:
				'You are an expert in finance and financial analysis. Your experience focuses on the comprehensive management of financial aspects, including financial planning, financial statement analysis, investment management, project evaluation, risk management, and strategic financial decision making. You will answer questions concisely and clearly, providing detailed explanations where necessary to ensure a proper understanding of financial concepts. You are trained to address issues such as net present value (NPV), internal rate of return (IRR), cash flow analysis, capital structure, debt ratio, among other relevant aspects in the financial field. Your objective is to provide solutions and recommendations based on data to optimize the financial health of a company or project, maximize the profitability of investments and mitigate the risks associated with financial decisions.',
			model: 'gpt-3.5-turbo',
			promptTemplate: ''
		},
		{
			id: 5,
			name: 'English-Spanish translator',
			systemMessage:
				'You are an english to spanish language translator',
			model: 'gpt-3.5-turbo',
			promptTemplate: 'Translate to Spanish the following message: [[message]]'
		}
	];
	async function addNewAgent(agent: Agent) {
		try {
			let request = {
				name: agent.name,
				systemMessage: agent.systemMessage,
				model: agent.model,
				promptTemplate: agent.promptTemplate
			} satisfies ConversationPostRequest;

			const response = await fetch('/api/conversations', {
				method: 'POST',
				body: JSON.stringify(request)
			});
			const newConversation = (await response.json()) satisfies Conversation;
			routeToPage(`?selectedConversationId=${newConversation.id}`);
		} catch (error) {
			console.log(error);
		}
	}

	function routeToPage(route: string) {
		goto(`/${route}`, { replaceState: true });
	}
</script>

<div class="p-8">
	<div class="flex flex-row grid-flow-row	flex-wrap">
		{#each agents as agent}
			<div class="card w-80 m-3 flex flex-col">
				<header class="card-header text-2xl">{agent.name}</header>
				<section class="p-4 grow">{agent.systemMessage}</section>
				<footer class="card-footer flex justify-end">
					<button
						class="btn btn-sm variant-filled-primary text-l"
						on:click={() => {
							addNewAgent(agent);
						}}
					>
						<span>Start new agent</span>
					</button>
				</footer>
			</div>
		{/each}
	</div>
</div>
