<script>
    import { onMount } from 'svelte';

    let tables = [];
    let selectedTable = null;
    let tableDetails = null;
    let isLoading = true;
    let detailsLoading = false;
    let error = null;
    let newTableName = '';
    let newRowData = {};

    const fakeTableDetails = {
        users: {
            columns: ['id', 'name', 'email'],
            rows: [
                { id: 1, name: 'Alice', email: 'alice@example.com' },
                { id: 2, name: 'Bob', email: 'bob@example.com' }
            ]
        },
        orders: {
            columns: ['order_id', 'product', 'amount'],
            rows: [
                { order_id: 101, product: 'Laptop', amount: 1200 },
                { order_id: 102, product: 'Phone', amount: 800 }
            ]
        }
    };

    async function fetchTables() {
        isLoading = true;
        error = null;
        setTimeout(() => {
            tables = Object.keys(fakeTableDetails);
            isLoading = false;
        }, 500);
    }

    async function fetchTableDetails(tableName) {
        detailsLoading = true;
        error = null;
        selectedTable = tableName;
        setTimeout(() => {
            if (fakeTableDetails[tableName]) {
                tableDetails = fakeTableDetails[tableName];
            } else {
                error = 'Table not found';
                tableDetails = null;
            }
            detailsLoading = false;
        }, 500);
    }

    function createTable() {
        if (!newTableName.trim()) return;
        fakeTableDetails[newTableName] = { columns: ['id'], rows: [] };
        tables = [...tables, newTableName];
        newTableName = '';
    }

    function addRow() {
        if (!selectedTable) return;
        const columns = tableDetails.columns;
        const newRow = {};
        columns.forEach(col => newRow[col] = newRowData[col] || '');
        tableDetails.rows = [...tableDetails.rows, newRow];
        newRowData = {};
    }

    function updateRow(index, column, event) {
        tableDetails.rows[index][column] = event.target.value;
    }

    function deleteRow(index) {
        tableDetails.rows = tableDetails.rows.filter((_, i) => i !== index);
    }

    function deleteTable() {
        if (!selectedTable) return;
        delete fakeTableDetails[selectedTable];
        tables = tables.filter(table => table !== selectedTable);
        selectedTable = null;
        tableDetails = null;
    }

    onMount(fetchTables);

    let theme = 'light';

    function toggleTheme() {
        theme = theme === 'light' ? 'dark' : 'light';
        document.documentElement.setAttribute('data-theme', theme);
        localStorage.setItem('theme', theme);
    }

    onMount(() => {
        const savedTheme = localStorage.getItem('theme') || 'light';
        theme = savedTheme;
        document.documentElement.setAttribute('data-theme', theme);
    });
</script>
<div>
    <button 
        class="btn absolute top-4 right-4 transition-colors duration-300"
        class:btn-neutral={theme === 'light'}
        class:btn-soft={theme === 'dark'}
        on:click={toggleTheme}
        >
            {theme === 'light' ? '🌙' : '☀️'}
    </button>
    <br>
    <br>
    <div class="grid md:grid-cols-4 gap-4 h-[80vh] p-4">
        <!-- Left Panel -->
        <div class="bg-base-200 shadow-lg rounded-xl p-4 overflow-auto md:col-span-1">
            <h2 class="text-lg font-bold mb-4">Tables</h2>
            {#if isLoading}
                <div class="text-gray-500 text-center">Loading tables...</div>
            {:else if error}
                <div class="text-red-500 text-center">{error}</div>
            {:else}
            <div class="mb-4 flex gap-2">
                <input type="text" bind:value={newTableName} placeholder="New Table Name" class="input input-bordered flex-1" />
                <button class="btn btn-info" on:click={createTable}>Create</button>
            </div>
                <ul class="space-y-2">
                    {#each tables as table (table)}
                        <button class="btn w-full {selectedTable === table ? 'btn-primary' : 'btn-outline'}" on:click={() => fetchTableDetails(table)}>
                            {table}
                        </button>
                    {/each}
                </ul>
            {/if}
        </div>

        <!-- Right Panel -->
        <div class="bg-base-100 shadow-lg rounded-xl p-4 overflow-auto md:col-span-3">
            {#if detailsLoading}
                <div class="text-gray-500 text-center">Loading table details...</div>
            {:else if error}
                <div class="text-red-500 text-center">{error}</div>
            {:else if tableDetails}
                <div class="flex justify-between items-center border-b pb-2">
                    <h3 class="text-xl font-semibold">{selectedTable} ({tableDetails.rows.length} rows)</h3>
                    <button class="btn btn-error btn-sm" on:click={deleteTable}>Delete Table</button>
                </div>

                <div class="overflow-x-auto mt-4">
                    <table class="table w-full border rounded-lg">
                        <thead>
                            <tr class="bg-base-300">
                                {#each tableDetails.columns as column}
                                    <th class="p-2">{column}</th>
                                {/each}
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            {#each tableDetails.rows as row, i}
                                <tr class="hover:bg-base-200">
                                    {#each tableDetails.columns as column}
                                        <td class="p-2 border">
                                            <input type="text" bind:value={row[column]} on:input={(e) => updateRow(i, column, e)} class="input input-sm w-full" />
                                        </td>
                                    {/each}
                                    <td>
                                        <button class="btn btn-error btn-sm" on:click={() => deleteRow(i)}>Delete</button>
                                    </td>
                                </tr>
                            {/each}
                        </tbody>
                    </table>
                </div>

                <div class="mt-4 p-4 bg-base-200 rounded-lg">
                    <h4 class="font-semibold mb-2">Add Row</h4>
                    <div class="grid grid-cols-4 gap-2">
                        {#each tableDetails.columns as column}
                            <input type="text" bind:value={newRowData[column]} placeholder={column} class="input input-bordered input-sm" />
                        {/each}
                        <button class="btn btn-primary btn-sm" on:click={addRow}>Add</button>
                    </div>
                </div>
            {:else}
                <div class="text-gray-500 text-center">Select a table to view details</div>
            {/if}
        </div>
    </div>
</div>

