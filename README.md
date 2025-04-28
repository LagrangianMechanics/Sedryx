# Sedryx

Sedryx is a Python library designed to enable the creation, manipulation, and visualization of quantum circuits with simplicity and expressiveness.
It introduces a system where circuits themselves are treated as first-class citizens, allowing modular, composable quantum programming with a focus on clarity, reusability, and creativity.

Sedryx is currently focused on the construction of quantum circuits. Future versions will incorporate simulation backends, enabling execution on professional-grade quantum circuit simulators, while maintaining the same elegant interface.

Installation

Sedryx is currently in development. Installation instructions will be provided with the first release.
Core Concepts

Sedryx's design philosophy revolves around a small number of powerful primitives:

Bits (qubits and classical bits) are the fundamental building blocks.
BitGroups enable multi-qubit operations through intuitive grouping.
Circuits are first-class objects and can be reused, composed, and treated as gates themselves.
Operators like >> and & provide a natural, fluid syntax for building circuits.
Creating Circuits

A circuit is created by specifying its bits either by number or by name:

from sedryx import Circuit, H, Cnot

# Create a Bell state
bell = Circuit(2)
x, y = bell.all_bits()
x >> H
(x & y) >> Cnot
Alternatively, you can name bits explicitly:

bell = Circuit('x', 'y')
To include classical bits, simply prefix the name with ':', or use negative integers:

# Two qubits and one classical bit
circuit = Circuit('x', 'y', ':c')

# Equivalent:
circuit = Circuit(2, -1)
Hidden bits (useful for intermediate computations) can also be created:

By appending ';' to a name, such as 'qh;'
By using floats instead of integers, such as 2.0
Hidden bits appear indented in the circuit diagram, distinguishing them from visible bits.

Gates, BitGroups, and Operators

In Sedryx, gates are applied using the >> operator:

x >> H
(x & y) >> Cnot
& groups multiple bits into a BitGroup, allowing multi-bit gates.
Multiple gates can be applied in sequence by passing a list:
x >> (H, X, Z)
Circuits themselves can be applied to groups of bits:

example = Circuit(4)
a, b, c, d = example.all_bits()
(a & b) >> bell
(c & d) >> bell
Thus, a circuit can behave like a reusable subroutine, with its structure seamlessly integrated into larger designs.

If one wishes to treat a circuit purely as a gate (without revealing its internal structure), the .as_gate method can be used:

bell_gate = bell.as_gate('Bell')
(a & b) >> bell_gate
This capability encourages the modular construction of complex circuits from smaller, understandable components.

Circuit Composition

Sedryx provides powerful composition tools:

Sequential composition: A >> B applies gate (or circuit) A followed by B to the same bits.
Parallel composition: A & B places gates (or circuits) side-by-side on separate bits.
For example:

# Create a simple parallel gate
H & X
These operations enable point-free circuit definitions, where bits need not be explicitly referenced:

# A Bell state in point-free style
bell = (H & I) >> Cnot
This expressive style emerged naturally from the library's design, highlighting Sedryx’s flexibility and underlying coherence.

Visualization

Sedryx supports both text-based and SVG-based visualization:

Text output provides a quick, human-readable diagram, suitable for standard terminals.
SVG output is ideal for Jupyter notebooks and provides a higher-quality, interactive visualization.
This dual approach ensures that circuits remain easy to inspect across different workflows.

Future Directions

Sedryx currently focuses on the creation and manipulation of circuits. Planned future developments include:

Integration with professional quantum circuit simulators as backends.
Extended support for measurements, classical control, and advanced quantum operations.
Optimization passes and circuit simplification tools.
Conclusion

Sedryx aims to provide a lightweight, expressive, and modular foundation for quantum circuit programming in Python.
Its emphasis on first-class circuits, compositionality, and minimal but powerful abstractions offers a fresh, flexible approach to designing quantum algorithms.

Whether you are experimenting with simple entanglement circuits or building the foundation for larger quantum algorithms, Sedryx strives to make the process elegant, intuitive, and fun.
