# Boxproof - LaTeX Macros for Natural Deduction Proofs

A LaTeX package for typesetting natural deduction proofs with boxes.

## About this Version

This is a **modified version** of Paul Taylor's original `boxproof.sty` package (1993).

- **Original author**: Paul Taylor (Imperial College London)
- **Original distribution**: https://www.paultaylor.eu/proofs/
- **Modified by**: Yunkai Zhang
- **Improvements**:
	- Added **clear, readable syntax** for proof boxes that's compatible with modern LaTeX editors;
    - added numbering for standalone boxes.
    - Built-in quick commands for `\premise` and `\asm`

## Installation

1. Download `boxproof.sty`
2. Place it in the same folder as your `.tex` file
3. Add `\usepackage{boxproof}` (or `\input{boxproof.sty}`) to your document preamble

Note: The modified version requires the `amssymb` package to be installed. Don't worry about this if you are using Overleaf.

## Quick Start

Here's a minimal example:

```latex
\documentclass{article}
\usepackage{boxproof}

\begin{document}

\begin{proofbox}
  \: A \land B \= \premise \\
  \: B \= \land E(1) \\
\end{proofbox}

\end{document}
```

Each line has the pattern:
```
\: [formula] \= [justification] \\
```

Note: more complicated usage with local variables are documented in the original file. It is not necessary for the purpose of the DMLR course.

## Types of Proof Boxes


### 1. Single-Column Boxes: `\open` ... `\close`

**Sample Usage**: →I, ∀I, ∃E, ¬E, ⊥I, derived laws

When you assume something and derive a conclusion within a box:

```latex
\begin{proofbox}
  \: A \= \premise \\
  \open
    \: B \= \asm \\
    \: A \land B \= \land I \\
  \close
  \: B \implic (A \land B) \= \implic I \\
\end{proofbox}
```
`\open` starts a box, `\close` ends it.

### 2. Case Analysis Boxes: `\openAlt` ... `\splitAlt` ... `\closeAlt`

**Sample Usage**: ∨E

When proving something by considering different cases, with boxes sharing a vertical line:

```latex
\begin{proofbox}
  \: A \lor B \= \premise \\
  \openAlt
    \: A \= \asm \\
    \: C \= \hbox{(somehow)} \\
  \splitAlt
    \: B \= \asm \\
    \: C \= \hbox{(somehow)} \\
  \closeAlt
  \: C \= \lor E(1, 2-3, 4-5) \\
\end{proofbox}
```

The `Alt` suffix means "alternatives" (cases). Use `\splitAlt` to separate cases.



### 3. (NOT USED IN DMLR COURSE) Independent Parallel Boxes: `\openInd` ... `\splitInd` ... `\closeInd`


When you prove two things separately side-by-side in a standalone flavour:

```latex
\begin{proofbox}
  \openInd
    \: A \= premise \\
    \: A \lor B \= \intro\lor \\
  \splitInd
    \: B \= premise \\
    \: A \lor B \= \intro\lor \\
  \closeInd
  \: (A \lor B) \land (A \lor B) \= \intro\land \\
\end{proofbox}
```

The `Ind` suffix means "independent" boxes. Use `\splitInd` to separate them.


### Summary: Which Command for Which Box?

| Natural Deduction Rule Examples | Box Type | Commands to Use |
|--------------------------|----------|-----------------|
| →I, ∀I, ∃E | Single box | `\open ... \close` |
| ∨E, ↔I | Case analysis | `\openAlt ... \splitAlt ... \closeAlt` |

**These commands are designed to be memorable and self-explanatory.** You won't have to look up cryptic bracket symbols!

## Line Syntax

Each proof line follows this pattern:

```latex
[label] \: [formula] \= [justification] \\
```

All parts except `\\` are optional:

- **Label** (optional): `\label{myline}` or `\"myline"` - reference later with `\ref{myline}` if you need to use this line multiple times or you hate looking up line numberings
- **`\:`** starts your formula
- **Formula**: The proposition you're asserting
- **`\=`** starts your justification
- **Justification**: How you derived this line. Wrap with `\hbox{}` for ordinary English texts if needed
- **`\\`** ends the line (required!)

### Example with labels:

```latex
\begin{proofbox}
  \"1" \: A \land B \= \premise \\
  \: A \= \land E(\ref{1}) \\
  \"2" \: B \= \land E(\ref{1}) \\
  \: B \land A \= \land I(\ref{2},2) \\
\end{proofbox}
```

Note: `\ref{-}` references the previous line!

## Complete Example

Here's a proof of $(A \land B) \land C \implies A \land C$:

```latex
\begin{proofbox}
  \open
    \"premise" \: (A \land B) \land C \= \asm \\
    \: A \land B \= \land E(\ref{premise}) \\
    \: A \= \land E \\
    \: C \= \land E(\ref{premise}) \\
    \: A \land C \= \land I(3, 4) \\
  \close
  \: ((A \land B) \land C) \implic (A \land C) \= \implic I(1, 5) \\
\end{proofbox}
```

## Helpful Commands
<!-- 
The package provides convenient notations for inference rules:

```latex
\intro\land    % ∧I (conjunction introduction)
\elim\land     % ∧E (conjunction elimination)
\intro\lor     % ∨I (disjunction introduction)
\elim\lor      % ∨E (disjunction elimination)
\intro\implic  % →I (implication introduction)
\elim\implic   % →E (implication elimination, modus ponens)
\intro\forall  % ∀I (universal introduction)
\elim\forall   % ∀E (universal elimination)
\intro\exists  % ∃I (existential introduction)
\elim\exists   % ∃E (existential elimination)
``` -->

Quantifiers with nice spacing:

```latex
\All x:X. \phi(x)     % ∀x:X. φ(x) (universal quantifier)
\Some x:X. \phi(x)    % ∃x:X. φ(x) (existential quantifier)
```

## Common Issues

**Problem**: Boxes don't appear / strange errors
**Solution**: Make sure you have `\begin{proofbox}...\end{proofbox}` around the whole proof

**Problem**: Forgot which command to use?
**Solution**:
- Single box? `\open ... \close`
- Cases? `\openAlt ... \splitAlt ... \closeAlt`

**Problem**: Spacing looks weird
**Solution**: Make sure you have `\\` at the end of each line

## Legacy Syntax (Not Recommended for this case)

The original package used bracket-based commands: `\[`, `\]`, `\(`, `\)`, `\*`, `\+`. These still work for backwards compatibility, but **we strongly recommend using the new syntax** (`\open`, `\close`, etc.) because:

- Hard to remember which bracket does what
- Easy to mix up `\*` and `\+`
- If you are writing in modern Overleaf they throw you errors, although the code still compiles

## Getting Help

- The original package has more advanced features - check `boxproof.sty` for details
- For questions about this modified version: issues and pull requests are welcomed

## License

- **Original work**: Paul Taylor (distributed publicly without explicit license)
- **Modified version**: MIT License (see LICENSE file)

## Credits

This package exists thanks to Paul Taylor's original work creating elegant proof macros for the LaTeX community. The modifications add clearer syntax to make the package more accessible for students learning natural deduction.

Also I consulted the fitch-style natural deductions proofs package for naming
