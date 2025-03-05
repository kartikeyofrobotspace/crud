# Svelte Table Manager

A simple Svelte-based application for managing tables and their data. This app allows you to create, view, update, and delete tables and rows in a user-friendly interface.

## Features

- **Table Management**:
  - View a list of available tables.
  - Create new tables with custom names.
  - Delete existing tables.

- **Row Management**:
  - Add new rows to a selected table.
  - Edit existing rows directly in the table.
  - Delete rows from the table.

- **Dynamic Data Loading**:
  - Simulated asynchronous loading for tables and table details.
  - Error handling for invalid table names.

- **Theme Switching**:
  - Toggle between light and dark themes.
  - Persist theme preference using `localStorage`.


## Usage

### Table Operations

1. **View Tables**:
   - The left panel displays a list of available tables.
   - Click on a table name to load its details in the right panel.

2. **Create a New Table**:
   - Enter a name for the new table in the input field under the "Tables" section.
   - Click the "Create" button to add the table.

3. **Delete a Table**:
   - Select a table to view its details.
   - Click the "Delete Table" button in the top-right corner of the table details section.

### Row Operations

1. **Add a New Row**:
   - Scroll to the "Add Row" section in the right panel.
   - Enter values for each column and click the "Add" button.

2. **Edit a Row**:
   - Click on any cell in the table to edit its value.
   - Changes are saved automatically.

3. **Delete a Row**:
   - Click the "Delete" button in the "Actions" column of the row you want to remove.

### Theme Switching

- Click the 🌙 or ☀️ button in the top-right corner to toggle between light and dark themes.

## Code Structure

- **State Management**:
  - `tables`: List of available tables.
  - `selectedTable`: Currently selected table.
  - `tableDetails`: Details (columns and rows) of the selected table.
  - `isLoading`, `detailsLoading`: Loading states for tables and table details.
  - `error`: Error messages for invalid operations.

- **Functions**:
  - `fetchTables`: Simulates fetching a list of tables.
  - `fetchTableDetails`: Simulates fetching details of a selected table.
  - `createTable`: Adds a new table to the list.
  - `addRow`: Adds a new row to the selected table.
  - `updateRow`: Updates a row's value in real-time.
  - `deleteRow`: Removes a row from the table.
  - `deleteTable`: Deletes the selected table.
  - `toggleTheme`: Switches between light and dark themes.

- **UI Components**:
  - Left Panel: Displays the list of tables and allows table creation.
  - Right Panel: Shows details of the selected table, including rows and actions.

## Styling

The app uses a modern UI framework with responsive design. Key features include:
- Grid layout for organizing panels.
- Buttons, inputs, and tables styled for clarity and usability.
- Smooth transitions for theme switching.

## Notes

- This app uses simulated data (`fakeTables` and `fakeTableDetails`) for demonstration purposes.
- All operations are performed in-memory and do not persist after refreshing the page.
