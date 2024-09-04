```python


import random
import matplotlib.pyplot as plt

def dot_product(vector1, vector2):
    return sum(x * y for x, y in zip(vector1, vector2))

def train_adaline(inputs, targets, weights, bias, learning_rate, num_epochs):
    accuracy_per_epoch = []

    for epoch in range(num_epochs):
        correct_predictions = 0

        total_squared_error = 0

        for i in range(len(inputs)):
            input_vector = inputs[i]
            target = targets[i]

            # Compute net input
            net_input = dot_product(input_vector, weights) + bias
            
            # Compute the error
            error = target - net_input 
            
            # Compute squared error
            squared_error = error ** 2
            total_squared_error += squared_error

            # Find change in weight, bias using LMS
            del_weight = [learning_rate * error * x for x in input_vector]
            del_bias = learning_rate * error

            # Update weight, bias
            for i in range(len(weights)):
                weights[i] += del_weight[i]
            bias += del_bias

            # Determine if the prediction was correct
            predicted_output = 1 if net_input >= 0 else 0
            if predicted_output == target:
                correct_predictions += 1

        # Calculate accuracy for this epoch
        accuracy = correct_predictions / len(inputs) * 100
        accuracy_per_epoch.append(accuracy)

    return weights, bias, accuracy_per_epoch

# Hyperparameters
learning_rate = 0.1
num_epochs = 30

# Gate inputs and target outputs
inputs = [
    [0, 0],
    [0, 1],
    [1, 0],
    [1, 1]
]

# Targets for AND and OR gates
targets_and = [0, 0, 0, 1]
targets_or = [0, 1, 1, 1]

weights_and, bias_and = [random.uniform(-1, 1) for _ in range(2)], random.uniform(-1, 1)
weights_or, bias_or = [random.uniform(-1, 1) for _ in range(2)], random.uniform(-1, 1)

print("Initial random weights and biases:")
print("AND Gate - Weights:", [f"{w:.2f}" for w in weights_and], f"Bias: {bias_and:.2f}")
print("OR Gate - Weights:", [f"{w:.2f}" for w in weights_or], f"Bias: {bias_or:.2f}")

#  Init between [-1, 1]
weights_and, bias_and, accuracies_and = train_adaline(inputs, targets_and, weights_and, bias_and, learning_rate, num_epochs)
weights_or, bias_or, accuracies_or = train_adaline(inputs, targets_or, weights_or, bias_or, learning_rate, num_epochs)

print("\nfinal results:")
print("and gate - weights:", [f"{w:.2f}" for w in weights_and], f"bias: {bias_and:.2f}")
print("or gate - weights:", [f"{w:.2f}" for w in weights_or], f"bias: {bias_or:.2f}")

# Plotting accuracy vs epoch for AND and OR gates
plt.figure(figsize=(12, 5))

# Plot for AND gate
plt.subplot(1, 2, 1)
plt.plot(range(1, len(accuracies_and) + 1), accuracies_and)
plt.xlabel('Epoch')
plt.ylabel('Accuracy (%)')
plt.title('Accuracy vs Epoch for AND Gate')
plt.grid(True)

# Plot for OR gate
plt.subplot(1, 2, 2)
plt.plot(range(1, len(accuracies_or) + 1), accuracies_or)
plt.xlabel('Epoch')
plt.ylabel('Accuracy (%)')
plt.title('Accuracy vs Epoch for OR Gate')
plt.grid(True)

plt.tight_layout()
plt.show()
```