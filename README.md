# EternaMind
Creating an AI assistant that can "remember" conversations requires building a context-aware system.
import json

class AIAssistant:
    def __init__(self):
        # Initialize memory as a dictionary
        self.memory = {}
        self.load_memory()

    def load_memory(self, file_path="memory.json"):
        """Load memory from a JSON file."""
        try:
            with open(file_path, 'r') as file:
                self.memory = json.load(file)
            print("Memory loaded successfully!")
        except FileNotFoundError:
            print("No memory file found. Starting with a clean memory.")
        except json.JSONDecodeError:
            print("Error in memory file. Starting fresh.")
            self.memory = {}

    def save_memory(self, file_path="memory.json"):
        """Save memory to a JSON file."""
        with open(file_path, 'w') as file:
            json.dump(self.memory, file)
        print("Memory saved successfully!")

    def clear_memory(self):
        """Clear the assistant's memory."""
        self.memory = {}
        print("Memory cleared!")

    def remember(self, key, value):
        """Add information to memory."""
        self.memory[key] = value
        print(f"Remembered: {key} -> {value}")

    def recall(self, key):
        """Recall information from memory."""
        return self.memory.get(key, "I don't remember that.")

    def chat(self):
        """Main chat loop."""
        print("Hello! I am your AI Assistant. Type 'exit' to quit, 'clear' to reset memory.")
        while True:
            user_input = input("You: ")
            if user_input.lower() == 'exit':
                print("Goodbye!")
                self.save_memory()
                break
            elif user_input.lower() == 'clear':
                self.clear_memory()
                continue

            if user_input.startswith("Remember "):
                _, key, value = user_input.split(" ", 2)
                self.remember(key, value)
            elif user_input.startswith("What do you remember about "):
                _, _, key = user_input.split(" ", 4)
                print(f"AI: {self.recall(key)}")
            else:
                print(f"AI: Sorry, I don't understand '{user_input}'. Try 'Remember [key] [value]' or 'What do you remember about [key]'.")
