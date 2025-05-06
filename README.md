# **Sedry Diophasisx**  
*A Python Library for First-Class Quantum Circuit Construction*  

---

Sedryx is a lightweight, expressive framework designed to treat quantum circuits as first-class citizens in Python. Inspired by functional programming idioms and operator overloading, Sedryx enables you to build, compose, and visualize quantum circuits with concise, natural syntax. Although a simulator backend is still under development, Sedryx seamlessly integrates with popular Python-based quantum simulators in the near future.

## Key Concepts and Design Philosophy

1. **Circuits as Objects**  
   Every `Circuit` in Sedryx is a reusable object. Once defined, circuits may be applied like gates, combined, or even nested without manual re-construction of their internal structure.

2. **Operator Overloading for Clarity**  
   - **`>>`** applies a bit, bit group, or circuit to a gate.  
   - **`&`** groups bits or composes circuits/gates in parallel.  
   - **Lists** allow sequential application of multiple gates to the same bit.  

3. **Flexible Bit Naming and Ordering**  
   - Positional arguments (integers) or explicit labels (strings) name qubits and classical bits.  
   - Negative integers designate classical bits; floats and trailing semicolons (`;`) mark hidden bits for indented display.  
   - The order of specification determines the layout in visualizations.

4. **Extensibility via `.as_gate()`**  
   Convert any circuit into a gate object with a custom name, enabling higher-order circuit design (e.g., defining parameterized transforms like QFT).

## Installation

```bash
pip install sedryx
```

*Note: A simulation backend is forthcoming; currently, Sedryx focuses on circuit construction and visualization.*

## Quick Start

```python
from sedryx import Circuit, H, X, Z, Cnot, I

# 1. Create a Bell pair
bell = Circuit(2)
q0, q1 = bell.all_bits()
q0 >> H
(q0 & q1) >> Cnot

# 2. Reuse the Bell circuit in a 4-qubit circuit
example = Circuit(4)
a, b, c, d = example.all_bits()
(a & b) >> bell
(c & d) >> bell

# 3. Treat the Bell circuit as a gate
bell_gate = bell.as_gate('Bell')
(a & b) >> bell_gate
```

## Core API

### `Circuit(*bits)`
- **Arguments**:  
  - Positive `int` or `str` for qubits;  
  - Negative `int` or `str` prefixed with `':'` for classical bits;  
  - `float` or trailing semicolon (`'name;'`) to mark hidden bits.
- **Returns**: A new `Circuit` object.

### Bit Grouping and Application
```python
x, y = bell.all_bits()
(x & y) >> Cnot      # Two-qubit CNOT gate
x >> (X, H, Z)       # Sequence of gates on qubit x
```

### Circuit Composition
```python
# Parallel composition
double_bell = bell & bell   # 4-qubit circuit (Bell ⊗ Bell)

# Sequential composition of gates as circuits
composed = X >> H           # Returns a 1-qubit circuit: [X on q0] then [H on q0]
```

### Defining Custom Gates
```python
def QFT(n):
    qft = Circuit(n)
    qubits = qft.all_bits()
    # ... construct QFT logic ...
    return qft.as_gate(f'QFT({n})')

# Use it like any gate:
(c0 & c1 & c2) >> QFT(3)
```

## Visualization

Sedryx offers two rendering modes:

- **SVG output** (ideal for Jupyter notebooks): crisp, scalable diagrams with hidden qubits indented.  
- **ASCII art**: plain-text diagrams for terminal use (functional, though less detailed).

```python
from sedryx.visual import draw_svg, draw_ascii

print(draw_ascii(bell))    # ASCII diagram
display(draw_svg(bell))    # SVG in Jupyter
```

## Roadmap

- **Simulator Backend**: Integration with Python-based quantum simulators (e.g., Qiskit, Cirq).  
- **Parameterised Gates**: Native support for parameter sweeps and symbolic rotations.  
- **Advanced Layouts**: Automatic qubit mapping and wire layout optimization.  
- **Gate Libraries**: Prebuilt higher-level transforms (e.g., QFT, Grover’s diffuser).

## Contributing

Contributions are warmly welcomed. Please fork the repository on GitHub, submit issues for feature requests or bugs, and open pull requests. Ensure adherence to the existing code style and include tests for new functionality.

## License

Sedryx is released under the MIT License. See [LICENSE](LICENSE) for details.

---

Harness the expressive power of Sedryx to prototype, compose, and visualize quantum algorithms with ease—treating circuits truly as first-class citizens in Python.

---
## Acknowledgment

README generated with assistance from ChatGPT; all code and design decisions are my own.
