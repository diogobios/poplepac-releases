# PoplePAC — 3D Molecular Editor

**PoplePAC** is an interactive molecular editor inspired by classic tools like *HyperChem* and *Avogadro*. Built with **Python**, **PySide6**, and **VTK**, it provides a comprehensive environment for building, visualizing, analyzing, and computing chemical structures — from isolated molecules to periodic crystals.

---

## Key Features

### 3D Visualization
- **Rendering Modes:** **Ball & Stick**, **CPK** (Van der Waals), **Licorice**, **Wireframe**.
- **VTK Engine:** Dynamic lighting (key, fill, back lights), gradient background with toolbar color swatch.
- **Smart Labels:** `vtkBillboardTextActor3D` that auto-positions outside atom spheres, always facing the camera.
- **Molecular Axes:** Toggle XYZ axes at the molecular centroid — configurable length and cylinder/cone styling.
- **Dynamic Highlighting:** Hover (cyan) and selection (gold) with emphasized borders.
- **Measurement Visualization:** Distance lines, angle arcs, and dihedral planes with floating 3D labels.
- **ESP Surface:** Electrostatic potential mapped onto electron-density isosurfaces or VDW surfaces with scalar bar, configurable opacity and grid resolution, auto-adapting text color based on background luminance.
- **Molecular Orbital Visualization:** Positive/negative isosurfaces for individual MOs with configurable isovalue, colors, and opacity.
- **Spin Density Visualization:** Alpha − beta electron density rendered as ± isosurfaces alongside MO and ESP modes.
- **Visual Style Control:** Dedicated dock for high-level customization of atoms and labels. Adjust font families (standard VTK Arial, Courier, Times), font sizes, bold styles, colors, and opacity with real-time updates.
- **Interactive Label Movement [Move Mode]:** Unique feature to interactively reposition labels (atom names, measures, coordination symmetry) in 3D space. Click and drag labels with the mouse; displacements are preserved in screen-space regardless of camera rotation or zoom.
- **Contextual UI feedback:** Cursor changes (Pointing Hand on hover, Closed Hand on drag) and automatic color adaptation for maximum readability.
- **One-Click Reset:** Factory reset buttons to restore manual label displacements and styling to original defaults.
- **Theme System:** Dark, Light, and System-adaptive themes (View > Theme) with live switching across the entire application.

### Building Tools
- **Add Atom [A]:** Drag from an atom to create a new bonded atom; click empty space to place an isolated atom; click a bond to cycle bond order (Single → Double → Triple → Remove); click an existing atom to change its element.
- **Aromaticity Detection:** **Shift+Double Click** on any bond within a ring to automatically detect the shortest cycle (BFS) and convert all its bonds to aromatic order (1.5).
- **Bond picking:** Fuzzy bond detection picks bonds even when clicking between atoms in multi-bond regions.
- **Element selector:** Full periodic table — all 118 elements with CPK colors, radii, and default valences.
- **Z-Matrix Editor (Build > Z-Matrix Editor):** Table-based interactive editor for internal coordinates (bond distances, bond angles, dihedral angles). Supports integer indices and atom name references (`na`, `nb`, `nc`), live Cartesian coordinate preview, and full Undo/Redo via `SetZmatCommand`.
- **Visual Style:** Dock for real-time appearance customization, label formatting, and interactive displacement toggles.

### Coordination Chemistry Analysis
- **Automatic Metal Detection:** Identifies 80+ metal elements (alkali, alkaline earth, transition, lanthanide, actinide, post-transition).
- **Ligand Identification:** Distance-based with configurable covalent radius tolerance, or bond-connectivity-based using explicit molecular graph.
- **Continuous Shape Measures (CShM):** Native implementation using Kabsch alignment + exhaustive permutation search + Hungarian algorithm. No external SHAPE dependency. Validated against SHAPE 2.1 (0.000 Å mean difference on 39 CSD structures).
- **60+ Ideal Polyhedra:** Exact SHAPE 2.1 coordinates for CN=2 through CN=12 (octahedral, tetrahedral, square planar, trigonal bipyramid, square antiprism, muffin, etc.).
- **Coordination Dock:** Metal selector, geometry classification with symmetry labels (e.g., "CSAPR-9 (C4v)"), CShM values, full ranking, M-L distances, and HTML report export with SVG bar charts.
- **Atom Ligands Filtering:** Checkbox to isolate metal + coordinated ligand atoms in the 3D scene, hiding non-coordinated atoms, centroids, and irrelevant bonds for focused analysis.
- **3D Polyhedron Visualization:** VTK Delaunay3D convex hull with configurable opacity, color, and edge styling.

### Geometry Optimization
- **Interactive Auto-Optimize [Auto Checkbox]:** Live energy minimization using RDKit (MMFF94/UFF/UFF-Ln) that runs continuously. Allows users to click and drag atoms in 3D space, which act as temporary fixed points while the rest of the molecule elastically relaxes around the mouse cursor.
- **MMFF94 / MMFF94s / UFF:** RDKit force fields with selective optimization (fixed atoms) and iterative progress reporting.
- **UFF-Ln:** Dedicated lanthanide UFF with distance/angle/VDW constraints using parameters optimized against 1658 CSD structures. Supports Eu, Gd, Tb, Dy.
- **MOPAC:** PM7, PM6, AM1, RM1, PM3, MNDO, MINDO/3 semi-empirical methods.
- **xTB:** GFN2-xTB, GFN1-xTB, GFN0-xTB, GFN-FF with ALPB implicit solvent (13 solvents), charge/multiplicity control, and selective optimization.

### Measurement & Analysis
- **Measure Tool [E]:** Distance (2 atoms), Angle (3 atoms), Dihedral (4 atoms) with editable values in the tool panel.
- **Centroids [Ctrl+K]:** Virtual points at the center of mass of a selection.
- **Point Group Detection:** Automatic Schoenflies point group displayed in the status bar and available via Build > Analyze Symmetry.

### Fragment Library
- **51 fragments** across 9 categories — alkyl, aromatic, functional groups, betadiketones, coordination ligands, MOF linkers, organometallic nodes, biomolecules, solvents.
- **PM6-optimized geometries** with MOL2 Sybyl atom types and Mulliken charges.
- **Anchor-based insertion** with automatic valence management, overlap removal, and smart bond detection.
- **Independent fragment manipulation:** Right-drag to rotate, Shift+Right-drag to translate, Ctrl+Right-drag to spin.
- **Interactive ghost preview** with snapping, collision detection, and anchor cycling (R/T/Z keys).

### Reaction Path Viewer (Tools > Reaction Path)
- **Multi-source parsing:** Reads MOPAC IRC, DRC, and optimization trajectories directly from `.out` files.
- **Frame-by-frame navigation** and **smooth animation** with configurable playback speed.
- **Energy profile chart:** Interactive matplotlib plot of potential energy along the reaction coordinate with clickable data points.
- **Ghost overlay:** Superimposes the previous frame as a translucent ghost for visual comparison.
- **Reactant / Product Viewer:** Dual embedded 3D viewer (reactant on top, product on bottom) for side-by-side comparison.
- **Export:** Trajectory to XYZ, PDB, individual frames as PNG, and animated GIF/MP4.
- **CLI access:** `poplepac reaction <file> [--export <out.xyz>]`

### Solid-State Builder (Tools > Solid-State Builder)
- **Crystal I/O:** Reads and writes CIF, MMCIF, and PDB periodic structures via the `CrystalIO` engine.
- **Symmetry Engine:** Space group detection, symmetry operator application, and equivalent site generation.
- **Supercell Generation:** Expand unit cells along arbitrary (na × nb × nc) directions with background-thread progress reporting.
- **Surface & Slab Builder:** Cleave planes by Miller indices (hkl), control slab thickness and vacuum layer.
- **Cluster Extraction:** Cut a spherical or box cluster from a periodic structure for molecular calculations.
- **Structure Editor:** Atom addition/deletion, site substitution, and lattice parameter editing.
- **Contact Analyzer:** Identify intermolecular contacts below VdW sum + tolerance threshold.
- **Plane & Slice Engine:** Visualize crystallographic planes and slice the structure along any (hkl).
- **Void Analyzer:** Detect and visualize cavities and pore volumes within crystal structures.
- **Unified 3D Viewer:** All solid-state operations render directly into the main PoplePAC VTK scene.
- **Activity Bar integration:** Accessible via the VS Code-style activity bar on the left edge.

### ORCA Input Builder (Tools > ORCA Input Builder)
- **Metadata-driven:** Block/keyword catalog loaded from `catalog.json` covering ORCA 6.1 grammar.
- **Three-pane GUI:** Block palette (with category grouping and incremental search) | Dynamic block form editor | Live `.inp` preview with validation messages.
- **Presets:** One-click application of common calculation templates (single-point, geometry optimization, TD-DFT, etc.) stored in `presets.json`.
- **Validation levels:** Structural, semantic, and domain checks produce colored `ValidationIssue` messages in real time.
- **Coordinate formats:** Inline XYZ, internal (Z-matrix), GZMT, and external XYZ file reference.
- **Molecule integration:** Reads the current PoplePAC molecule automatically (element, charge, multiplicity).
- **Export:** Writes clean, well-formatted `.inp` files ready for submission.

### Vibrational Mode Viewer
- **Normal mode visualization:** Animated displacement arrows rendered via `vtkGlyph3D` directly on the molecule.
- **Animation control:** Play/pause, adjustable amplitude and playback speed.
- **IR spectrum plot:** Convolved IR spectrum (Lorentzian/Gaussian) rendered with matplotlib in an embedded canvas.
- **Mode selector:** Dropdown listing all normal modes with their frequencies; skips translation/rotation modes.
- **Arrow styling:** Configurable color, scale factor, and tip/shaft proportions.
- **Parsed from:** MOPAC and ORCA output files via the quantum output parsers.

### File I/O
- **Import:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.mmcif`, `.cml`, `.out`, `.orcout` (ORCA/MOPAC) — via OpenBabel with automatic bond perception.
- **Export:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.smi`, `.pdbqt`, `.gro`, `.tmol`.
- **Save (Ctrl+S):** Saves directly to current file; Save As (Ctrl+Shift+S) with format chooser.
- **Recent Files:** Last 10 opened files in File > Open Recent.
- **Drag & Drop:** Drop files directly into the 3D viewport.
- **Background Loading:** Large files (>2000 atoms) load in background thread with progress dialog and switch to optimized read-only mode.
- **Auto-Save:** Saves molecule state every 5 minutes. Crash recovery dialog on next startup.

### Quantum Output Viewers
- **MOPAC Viewer:** Real-time output streaming, settings editor (charge, multiplicity, keywords), wavefunction parsing from `.aux` files, MO/ESP/Spin Density visualization, and export of calculation files.
  - **Keywords Syntax Highlighting:** Live `QSyntaxHighlighter` color coding distinguishing keywords (teal), Cartesian coordinates (purple), and Z-matrix connectivity indices (orange).
  - **Dynamic Scene Sync:** Keyword preview stays 100% synchronized with 3D scene modifications (atom additions, deletions, coordinate shifts) and Undo/Redo actions in real time.
- **ORCA Viewer:** Wavefunction parsing from `.out` / `.orcout` files (basis set + MO coefficients + unrestricted alpha/beta), MO/ESP/Spin Density visualization, and GBW metadata detection.
- **xTB Viewer:** xTB output streaming, settings editor (method, solvent, charge/multiplicity), and MO/ESP visualization.
- **Excited States Analysis (ORCA TD-DFT):** Fragment-based characterization of singlet and triplet excited states. Decomposes MO contributions by ligand fragment with absorption spectrum plotting (Gaussian/Lorentzian convolution, wavelength/energy/eV units), interactive stick tooltips showing transition weights and orbital labels, CSV export of convolved spectrum, and HTML/text report generation.
- **Structure Alignment & RMSD:** Toggle between Input/Output geometry with one click. Merge (RMSD) mode aligns initial and final structures via Kabsch algorithm and displays the aligned overlay with RMSD label directly in the 3D scene.
- **Embedded Search Bar:** Collapsible Find bar with keyword highlighting (yellow matches, red active) and Previous/Next navigation in all quantum output panels.

### Command-Line Interface (CLI)
A standalone `poplepac` CLI runs without loading any graphical libraries:

```
poplepac info      <file>           — Structural info (formula, atoms, bonds, charge, multiplicity)
poplepac symmetry  <file>           — Point group (Schoenflies) analysis
poplepac coordination <file>        — Metal centers and coordination geometry classification
poplepac reaction  <file> [--export <out.xyz>]  — Reaction path analysis and XYZ export
```

### User Experience
- **Undo/Redo:** Full command history across all editing operations.
- **Quick Element Substitution:** Hover an atom and type Shift+letters (e.g., Shift+Cl for chlorine).
- **Dirty Flag:** Window title shows `*` when there are unsaved changes. Prompts on close/new/clear only when dirty.
- **Multiple Windows:** File > New opens an independent new PoplePAC window.
- **Save Image:** Capture the VTK viewport as PNG/JPEG from the Surface dock or via Ctrl+E.
- **Activity Bar:** VS Code-style icon bar on the left edge for quick access to panels (Fragment Library, Solid-State Builder, Reaction Path, MOPAC/ORCA/xTB outputs, Coordination Analysis).
- **Keyboard Shortcuts:** Single-key tool switching (N/S/A/D/M/E/I) and standard shortcuts (Ctrl+Z/Y/C/V/I/H/G/K/L/0).

---

## Keyboard Shortcuts

| Key | Action |
| :--- | :--- |
| **N** | Navigate Tool |
| **S** | Select Tool |
| **A** | Add Atom Tool |
| **D** | Delete Tool |
| **M** | Move Tool |
| **E** | Measure Tool |
| **I** | Insert Fragment |
| **Ctrl+Z / Y** | Undo / Redo |
| **Ctrl+C / V** | Copy / Paste |
| **Ctrl+S** | Save |
| **Ctrl+Shift+S** | Save As |
| **Ctrl+E** | Export Scene Image |
| **Ctrl+N** | New Window |
| **Ctrl+O** | Open File |
| **Ctrl+I** | Optimize Geometry |
| **Ctrl+H** | Add Hydrogens |
| **Ctrl+G** | Guess Bonds |
| **Ctrl+K** | Create Centroid |
| **Ctrl+L** | Toggle Labels |
| **Ctrl+0** | Reset Camera |
| **Ctrl+Shift+Y** | Toggle Molecular Axes |
| **Delete** | Remove Selection |
| **Escape** | Clear Selection |
| **Shift+Letter** | Quick Element Substitution |

---

## Supported Formats

**Import:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.mmcif`, `.cml`, `.out`, `.orcout` (ORCA/MOPAC)

**Export:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.smi`, `.pdbqt`, `.gro`, `.tmol`

---

## Project Architecture

- `poplepac/core` — Molecule/Atom/Bond/Centroid data models, periodic table (118 elements), fragment library, optimization engines (MMFF94, UFF, UFF-Ln, MOPAC, xTB), electronic structure (wavefunction, density, ESP, spin density), CShM coordination analysis, Cython acceleration.
- `poplepac/commands` — Command pattern for undo/redo across all editing operations.
- `poplepac/tools` — Interactive tools: navigate, select, add atom, delete, move, measure, insert fragment.
- `poplepac/coordination` — Metal detection, ligand identification, CShM computation (Kabsch+Hungarian+exhaustive), ideal polyhedron database (60+ SHAPE 2.1 geometries), geometry classification, Cython exhaustive search kernel.
- `poplepac/reaction` — Reaction path data model, MOPAC output parser, animation player, frame interpolator, energy chart, reactant/product viewer, trajectory exporter.
- `poplepac/solid_state` — Crystal structure models, space group symmetry engine, supercell generator, slab/surface builder, cluster extractor, contact/void/plane analyzers, CIF/MMCIF I/O, unified 3D scene integration.
- `poplepac/orca_input` — Metadata-driven ORCA 6.1 input builder: block catalog, builder, serializer, multi-level validator, molecule adapter, and three-pane GUI.
- `poplepac/view` — VTK scene management, 3D rendering (atoms, bonds, labels, orbitals, ESP surfaces, measurement overlays, polyhedra, vibrational arrows).
- `poplepac/ui` — PySide6 GUI: main window, activity bar, docks (Tool Settings, Properties, Visual Style, Analysis), quantum output panels (MOPAC, ORCA, xTB, Excited States, Vibrational), Z-Matrix editor, fragment dialog, diagram editor, theme manager.
- `poplepac/io` — File I/O via OpenBabel and RDKit, ORCA/MOPAC output parsing, cube file I/O.
- `poplepac/cli` — Pure CLI entry point (`poplepac info`, `symmetry`, `coordination`, `reaction`), fully decoupled from PySide6.
- `poplepac/utils` — Z-matrix utilities, math helpers.

---

## License
This project is for educational and research purposes in molecular modeling.
