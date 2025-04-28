# Sedryx

A Python library for quantum circuit creation with an elegant, intuitive syntax.

## Overview

Sedryx provides a clean, Pythonic interface for building quantum circuits. The library treats circuits as first-class citizens, allowing for easy composition, reuse, and manipulation of quantum circuits.

## Features

- **Intuitive circuit creation** using a natural syntax
- **Circuit composition** that enables reuse and nesting
- **First-class circuit objects** that can be manipulated, composed, and transformed
- **Flexible bit specification** for quantum and classical bits
- **Operator-based approach** for applying gates and joining circuits

## Installation

```bash
pip install sedryx
```

## Quick Start

### Creating a Simple Bell State

```python
# Create a Bell state circuit
bell = Circuit(2)  # Create circuit with 2 qubits
x, y = bell.all_bits()  # Get references to the qubits
x >> H               # Apply Hadamard to first qubit
(x & y) >> Cnot      # Apply CNOT gate with control on x and target on y
```

### Reusing Circuits

```python
# Create a larger circuit using the Bell state twice
example = Circuit(4)
a, b, c, d = example.all_bits()
(a & b) >> bell      # Reuse the bell circuit on first two qubits
(c & d) >> bell      # Reuse the bell circuit on second two qubits
```

## Core Concepts

### Circuit Creation

Circuits can be created with different bit specifications:

```python
# Using numbers (creates 2 qubits)
circuit = Circuit(2)

# Using strings (creates 2 qubits named 'x' and 'y')
circuit = Circuit('x', 'y')

# Mix of quantum and classical bits
# Creates 2 qubits and 1 classical bit
circuit = Circuit('x', 'y', ':c')

# Alternative notation for classical bits
circuit = Circuit(2, -1)  # 2 qubits, 1 classical bit
```

### Bit Operations

Bits can be grouped using the `&` operator to create BitGroups for multi-qubit gates:

```python
# Group bits for a 2-qubit gate
(x & y) >> Cnot

# Apply multiple gates to the same bit
x >> (H, X, Z)
```

### Circuit Composition

Circuits can be composed using operators:

```python
# Sequential composition using >>
new_circuit = X >> H  # Apply X, then H

# Parallel composition using &
two_qubit_gate = X & H  # Apply X to first qubit, H to second

# Compose entire circuits
larger_circuit = bell & bell  # Create a 4-qubit circuit from two bell states
```

### Circuit Reuse

Circuits can be reused or converted to gates:

```python
# Reuse a circuit directly
(a & b) >> bell

# Convert a circuit to a named gate
bell_gate = bell.as_gate('Bell')
x >> bell_gate  # Use like any other gate

# Create parameterized circuit functions
def QFT(n):
    qft = Circuit(n)
    # ... implementation ...
    return qft.as_gate(f'QFT({n})')
```

### Point-free Style

Circuits can be defined without explicit reference to bits:

```python
# Define bell circuit in point-free style
bell = (H & I) >> Cnot
```

## Examples

### Quantum Fourier Transform

```python
def QFT(n):
    qft = Circuit(n)
    bits = qft.all_bits()
    
    for i in range(n):
        bits[i] >> H
        for j in range(i+1, n):
            angle = 2 * math.pi / (2 ** (j-i+1))
            (bits[i] & bits[j]) >> Phase(angle)
    
    # Swap bits
    for i in range(n // 2):
        (bits[i] & bits[n-i-1]) >> Swap
        
    return qft.as_gate(f'QFT({n})')
```

### Grover's Algorithm

```python
def Grover(n, oracle):
    circuit = Circuit(n)
    qubits = circuit.all_bits()
    
    # Initialize in superposition
    for q in qubits:
        q >> H
    
    # Apply Grover iterations
    iterations = int(math.pi/4 * math.sqrt(2**n))
    for _ in range(iterations):
        # Oracle
        qubits >> oracle
        
        # Diffusion operator
        for q in qubits:
            q >> H
        for q in qubits:
            q >> X
        qubits >> MCZ  # Multi-controlled Z
        for q in qubits:
            q >> X
        for q in qubits:
            q >> H
    
    return circuit
```

## Coming Soon

- Backend integration with professional quantum circuit simulation libraries
- More built-in quantum algorithms
- Visualization tools

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
