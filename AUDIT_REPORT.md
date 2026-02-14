# Codebase Audit Report: The Hitchhiker's Guide to the Galaxy (ZIL Source)

## Executive Summary
This report outlines the findings from an audit of the source code for the Infocom game "The Hitchhiker's Guide to the Galaxy". The codebase is written in ZIL (Zork Implementation Language). The audit focuses on code quality, potential implementation issues, professionalism, and suggestions for modernization.

## 1. Security & Professionalism Findings

### 1.1. Unprofessional Naming Conventions
*   **Issue:** The routine `FUCKING-CLEAR` is defined in `verbs.zil` and used extensively throughout the codebase (`earth.zil`, `heart.zil`, `misc.zil`, etc.) to reset the parser state.
*   **Impact:** This is unprofessional and offensive.
*   **Recommendation:** Rename the routine to `RESET-PARSER` or `CLEAR-PARSER-CONTEXT`.

### 1.2. Offensive Content
*   **Issue:** The verb `RAPE` is defined in `syntax.zil` and implemented in `verbs.zil` (aliased to `V-KISS` which prints a refusal message).
*   **Impact:** The presence of this verb, even if refused, is inappropriate and offensive.
*   **Recommendation:** Remove the verb definition and implementation entirely.

### 1.3. Debug/Cheat Commands
*   **Issue:** The codebase contains debug commands like `V-$CHEAT`, `V-$VERIFY`, and `V-$DEBUG` (commented out). `V-$CHEAT` allows teleportation and state manipulation.
*   **Impact:** While useful for development, these should be disabled or stripped from a release build to prevent players from bypassing game content.
*   **Recommendation:** Ensure these are conditionally compiled or removed for release versions.

## 2. Implementation Issues & Bugs

### 2.1. Dead/Unreachable Code ("Bug #60")
*   **Issue:** In `globals.zil`, the `DARK-F` routine contains a probability check `<PROB ,WHALE-PROB>`. `WHALE-PROB` is initialized to 0 and never appears to be set to a non-zero value in the provided source. The block contains a message `<TELL "Bug #60" CR>`.
*   **Impact:** This logic is unreachable and serves no purpose.
*   **Recommendation:** Remove the dead code block.

### 2.2. Hardcoded Strings & Input Handling
*   **Issue:** The game relies heavily on hardcoded strings for all text output.
*   **Impact:** This makes localization or text modification difficult.
*   **Recommendation:** (Long-term) Move strings to a separate resource file if the ZIL architecture permits, or maintain a centralized string table.

### 2.3. Input Parser Limitations
*   **Issue:** The parser (`parser.zil`) uses a hardcoded list of names (`NAME?` routine) to identify characters.
*   **Impact:** Adding new characters requires modifying the parser logic.
*   **Recommendation:** Use object properties (flags) to identify characters instead of hardcoded names.

## 3. Suggestions for Improvements & Features

### 3.1. Modernization
*   **Undo/Oops:** Implement an `OOPS` command to allow players to correct a typo in the previous command or undo the last action (supported by later Z-machine versions).
*   **In-Game Hints:** Implement a context-sensitive hint system (similar to later Infocom titles) to reduce reliance on external materials.
*   **Object Highlighting:** Add a command (e.g., `HIGHLIGHT` or `OBJECTS`) to list all interactable objects in the current location to improve accessibility.

### 3.2. Code Cleanup
*   **Refactoring:** Remove unused variables and commented-out code blocks to improve readability.
*   **Standardization:** Standardize indentation and formatting (though ZIL has its own conventions).

## 4. Planned Actions
As part of this audit, the following immediate actions will be taken:
1.  Rename `FUCKING-CLEAR` to `RESET-PARSER` across the codebase.
2.  Remove `V-RAPE` from `syntax.zil` and `verbs.zil`.
3.  Remove the dead code block associated with `WHALE-PROB`.
