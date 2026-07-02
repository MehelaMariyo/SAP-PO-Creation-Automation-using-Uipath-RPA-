# SAP-PO-Creation-Automation-using-Uipath-RPA-


An enterprise-grade **UiPath RPA Workflow** designed to fully automate the end-to-end process of creating Purchase Orders (PO) in **SAP ERP** using structured data from **Excel**. 

This repository contains the complete automation pipeline, including environmental configuration steps, dynamic selector stabilization, transactional error-handling loops, and post-processing output extraction.

---

## 🗺️ Detailed Workflow Architecture

The automation is built linearly across 5 distinct execution stages, maximizing modularity and maintainability:

```text
[Stage 1: Pre-requisites] ──> [Stage 2: Authentication] ──> [Stage 3: Transaction Input]
                                                                        │
[Stage 5: Termination]   <──  [Stage 4: Post-Processing] <──────────────┘

```

## SAP PO Creation UiPath workflow 

![PO Creation Diagram](/artifacts/PO%20Creation%20Workflow%20Diagram.png)

1. Pre-requisites & Verification
    - Ensures SAP GUI Scripting interface is unlocked.
    - Server-Side validation via Profile Parameter `sapgui/user_scripting` (Transaction RZ11)
    - Client-Side validation via SAP GUI Options -> Accessibility & Scripting.

2. SAP Logon & Authentication Sequence
    - Launches the SAP Logon environment executable via the `SAP Logon` activity.
    - Targets the named connection profile and triggers the initial session screen. 
    - Executes secure credential injection via the native `SAP Login scope` (Client, User, Password, Language).

3. Transactions & Iterative Grid Injection
    - Triggers transaction code `ME21N` (Create Purchase Order) via a `Call Transaction` activity.
    - Enters an `Excel Process Scope` to parse transactional inputs from the source excell file.
    - Iterative Loop 1 (For Each Excel Row)
        - Evaluates Vendor continuity using a selective `if` string condition: `NOT String.IsNullOrEmpty(row("Vendor"))` 
        - Drops inside a sequential nested scope using `Table Cell Scope` mapping to the `First Empty Row`.
        - Maps variable cell text blocks into columns: `Material`, `Quantity`, and `Company code`.
        - Evatuate the `key` (`concat(Vender, SO)`) column in excell file `if` the next line is the same key.
        - Simulates the **Enter Key** action downstream of every record entry to validate the active line-item grid.
        - Fill the lead time date in the delivary note section of the interface for each line

4. Code Extraction & Output Binding
    - Executes a terminal `Click` event on the SAP 'Save' icon.
    - Captures the system's baseline acknowledgment note using the `Read Status Bar` activity.
    - Iterative Loop 2: Re-scans the active excel workflow data range and invokes the `Write Cell` activity to enter the created SAP PO number against its matching source transaction entries.

## 🛠️ Technical Stack & Core Activities

- RPA Engine: UiPath Studio 
- Target System: SAP GUI 
- Data Source: Microsoft Excel Workbook (.xlsx)
- Key UiPath Activities:
    - `SAP Logon` & `SAP Login` (Application Integration)
    - `Call Transaction` (Direct SAP Shortcutting)
    - `Table Cell Scope` (Dynamic structural data-grid targeting)
    - `Read Status Bar` (Direct extraction of system messages)
    - `Excel Process Scope` / `For Each Excel Row` / `Write Cell`/ `Click`/ `Use Application Browser`

    
