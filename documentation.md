# Coinbase CDP-Based AI Agents

Introduction

Coinbase CDP-Based AI Agents are intelligent agents designed to interact with the blockchain ecosystem, enabling the automation and execution of decentralized tasks. These agents can be integrated with smart contracts and perform operations without human intervention.

Use Cases

Transaction Executor: An agent that executes orders on the blockchain with predefined conditions.

Liquidity Management: Optimizes automated trading strategies on DeFi protocols.

DAO Automation: Allows approved proposals to be executed without manual intervention.

# System Architecture

Based AI Agents are built on three main layers:

User Interface (UI)

Web-based dashboard to configure agents and visualize their activity.

Intelligence Layer (AI Engine)

Decision-making engine based on machine learning models.

Integration with oracles to obtain external data.

Execution Layer (Blockchain Layer)

Set of smart contracts that execute transactions on the blockchain.

Compatible with Ethereum, Base and other EVM networks.

# Project Structure

The project is primarily composed of two key files:

1. agents.py: Contains the core functionality of the agent, including setting up the wallet and performing operations on the blockchain.

2. run.py: Defines the agent's modes of operation and handles user interaction.

# Explanation of run.py in Based AI Agents

This script is the entry point to run a Based AI Agent in different modes of operation. Mainly, it allows the agent to:

+ Operate autonomously (auto).

+ Interact in a chat with the user (chat).
+ Converse with another OpenAI-based agent (two-agent).

The agent uses the Swarm library to execute actions on the Base blockchain and receives instructions in natural language.

# 1. Imports and Initial Configuration

    import time
    import json
    from swarm import Swarm
    from swarm.repl import run_demo_loop
    from agents import based_agent
    from openai import OpenAI

+ time: Handling intervals between executions in standalone mode.

+ json: Processing structured responses.
swarm: Swarm AI client, which executes actions on the Base blockchain.

+ run_demo_loop: Swarm function to run interactive chat mode.

+ based_agent: Agent definition in agents.py.
OpenAI: OpenAI client to generate responses in two-agent mode.

# 2. Autonomous Mode: run_autonomous_loop

    def run_autonomous_loop(agent, interval=10):
        client = Swarm()
        messages = []

        print("Starting autonomous Based Agent loop...")

        while True:
            thought = (
                "Be creative and do something interesting on the Base blockchain. "
                "Don't take any more input from me. Choose an action and execute it now. "
                "Choose those that highlight your identity and abilities best."
            )
            messages.append({"role": "user", "content": thought})

            print(f"\n\033[90mAgent's Thought:\033[0m {thought}")

            response = client.run(agent=agent, messages=messages, stream=True)

            response_obj = process_and_print_streaming_response(response)
            messages.extend(response_obj.messages)

            time.sleep(interval)

# What does this function do?

1. Creates a Swarm client to run the agent.

2. Generates a "thought" that translates into a natural language instruction: "Do something interesting on the Base blockchain without asking for further instructions."

3. Executes the action with client.run(), processing the response in real time.

4. Waits interval seconds and repeats the process.

👉 This mode is ideal for running actions without user intervention.

# 3. AI-Agent Conversation Mode: run_openai_conversation_loop

    def run_openai_conversation_loop(agent):
        client = Swarm()
        openai_client = OpenAI()
        messages = []

        print("Starting OpenAI-Based Agent conversation loop...")

        openai_messages = [{
            "role": "system",
            "content": (
                "You are a user guiding a blockchain agent through various tasks on the Base blockchain. "
                "Engage in a conversation, suggesting actions and responding to the agent's outputs. "
                "Options include creating tokens, transferring assets, minting NFTs, and getting balances. "
                "Be unique and interesting."
            )
        }, {
            "role": "user",
            "content": "Start a conversation with the Based Agent and guide it through some blockchain tasks."
        }]

        while True:
            openai_response = openai_client.chat.completions.create(
                model="gpt-3.5-turbo", messages=openai_messages
            )

            openai_message = openai_response.choices[0].message.content
            print(f"\n\033[92mOpenAI Guide:\033[0m {openai_message}")

            messages.append({"role": "user", "content": openai_message})
            response = client.run(agent=agent, messages=messages, stream=True)
            response_obj = process_and_print_streaming_response(response)

            messages.extend(response_obj.messages)

            based_agent_response = response_obj.messages[-1]["content"] if response_obj.messages else "No response from Based Agent."
            openai_messages.append({
                "role": "user",
                "content": f"Based Agent response: {based_agent_response}"
            })

            user_input = input("\nPress Enter to continue the conversation, or type 'exit' to end: ")
            if user_input.lower() == 'exit':
                break


# ¿Qué hace esta función?

1. Crea un cliente Swarm y OpenAI.

2. Configura un sistema de conversación donde OpenAI guía al agente en tareas de blockchain.

3. Genera un mensaje de OpenAI en cada iteración, con instrucciones para el Based Agent.

4. Ejecuta la acción en Swarm y devuelve una respuesta.

5. Permite que el usuario decida si continuar o salir.

👉 Este modo permite que OpenAI ayude al Based Agent a explorar nuevas funciones en la blockchain.

# 4. User Interactive Mode

    def choose_mode():
        while True:
            print("\nAvailable modes:")
            print("1. chat    - Interactive chat mode")
            print("2. auto    - Autonomous action mode")
            print("3. two-agent - AI-to-agent conversation mode")

            choice = input("\nChoose a mode (enter number or name): ").lower().strip()

            mode_map = {
                '1': 'chat',
                '2': 'auto',
                '3': 'two-agent',
                'chat': 'chat',
                'auto': 'auto',
                'two-agent': 'two-agent'
            }

            if choice in mode_map:
                return mode_map[choice]
            print("Invalid choice. Please try again.")

# What does this feature do?
+ It has three available modes.

+ It allows you to choose between "chat", "auto" and "two-agent".

+ It validates user input.

# 5. Main Script Execution

    def main():
        mode = choose_mode()

        mode_functions = {
            'chat': lambda: run_demo_loop(based_agent),
            'auto': lambda: run_autonomous_loop(based_agent),
            'two-agent': lambda: run_openai_conversation_loop(based_agent)
        }

        print(f"\nStarting {mode} mode...")
        mode_functions[mode]()


    if __name__ == "__main__":
        print("Starting Based Agent...")
        main()


# What does this part do?

1. Call choose_mode to allow the user to choose a mode.

2. Run the corresponding function (chat, auto, or two-agent).

3. Start the program with main() when the script is executed.

# Security and Best Practices

Authentication: Implement authentication based on digital signatures to prevent unauthorized access.

Gas Optimization: Use strategies to minimize transaction costs in the Base network.

Monitoring: Use observability tools to track agent behavior.

# Frequently Asked Questions (FAQ)

Can I use a Based AI Agent on any blockchain?: Yes, they are compatible with any EVM network, such as Ethereum, Base, and Polygon.

How are transactions funded?: Agents can use a configured wallet to cover gas costs automatically.

# Conclusion

Coinbase CDP-Based AI Agent allow automating operations on the Base blockchain with a modular and flexible approach.

This project facilitates the exploration of transactions, smart contracts and decentralized tasks without manual intervention.