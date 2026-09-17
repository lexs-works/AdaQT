![AdaQT · IN DEVELOPMENT](https://img.shields.io/badge/AdaQT-WIP-c8a86b?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMCIgaGVpZ2h0PSIyMCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiNjOGE4NmIiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIj48Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIvPjxwYXRoIGQ9Ik0xMiA4djRsMiAyIi8+PC9zdmc+)

# Qt6Ada-Native

A clean, native, type-safe Ada 2005 binding library for Qt6.

Unlike other solutions, this project is built from the ground up following the Ada way. It sidesteps bloated runtime environments, heavy script interpreters, and indirect bridges entirely. It talks to Qt6 directly through a lightweight, high-performance C-ABI bridge.

### Key Features
* **🛡️ Ada Through and Through:** No raw `System.Address`, no unsafe pointers in user code. Everything strongly typed.
* **⚡ Zero-Interpreter Overhead:** No embedded Python, no PySide, no heavy wrappers under the bonnet. Pure native compilation.
* **🔒 Memory Safety & RAII:** Uses Ada's `Controlled` types to manage the lifecycle of Qt C++ objects automatically.
* **🔌 Type-Safe Signals & Slots:** Qt's signal-slot mechanism, wired natively into Ada access procedures.

---

### Architecture

The library uses a dual-layer "sandwich" architecture: a stable, high-performance C-ABI bridge that safely joins the C++ object model to Ada's tagged types.

<pre>
[ Your Ada Application Code ]
                │
                ▼
[ Ada High-Level API (Tagged & Controlled Types) ]
                │
                ▼
[ Low-Level Specifications (Interfaces.C / pragma Import) ]
                │
                ▼
(Stable C-ABI Linkage) [ C++ Wrapper / Bridge Layer (extern "C") ]
                │
                ▼
[ Qt6 Native Libraries (C++) ]
</pre>

---

### Signal & Slot Mechanics (The Native Way)

Signals and slots are handled purely in native code — no meta-object compilation (`moc`) overhead on the Ada side.

1. **C++ Side:** A thin extern "C" wrapper catches the Qt signal (via a standard modern C++ `QObject::connect` with lambda expressions).
2. **Bridge:** The lambda invokes a thin C-compatible function pointer (`proxy callback`).
3. **Ada Side:** The internal proxy safely restores the Ada object context and runs your strongly typed access procedure (`Slot`), passing the object by reference.

---

### Development Roadmap

- [ ] **Phase 1: Core Foundation & C-ABI Infrastructure**
    - [ ] Set up the `gprbuild` and CMake build pipelines.
    - [ ] Implement base memory management mapping (`Ada.Finalization` ↔ `C++ delete`).
    - [ ] Create the core architectural patterns for Object/Context dispatching.

- [ ] **Phase 2: Core Event Loop & Signals**
    - [ ] `QtCore` basic bindings (`QCoreApplication`, `QObject`).
    - [ ] Native signal-slot bridge implementation for basic event routing.
    - [ ] Type-safe string marshalling (`QString`  ↔ `Ada String` / `Unbounded_String`).

- [ ] **Phase 3: Basic Widgets (`QtWidgets` & `QtGui`)**
    - [ ] `QApplication` & `QWidget` lifecycle.
    - [ ] Form controls with basic signal bindings:
        - [ ] `QPushButton` (with clicked signal).
        - [ ] `QLabel`.
        - [ ] `QLineEdit` (with `textChanged` signal and string marshalling).
    - [ ] Layout management (`QVBoxLayout`, `QHBoxLayout`).

- [ ] **Phase 4: Advanced Elements (Future)**
    - [ ] Menus, dialogs, and windows (`QMenuBar`, `QDialog`, `QMainWindow`).
    - [ ] Basic `QPainter` layout for custom drawing.