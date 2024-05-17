<script>
    import { onMount } from "svelte";
    import { sortKey, sortDirection } from "./stores";

    let planets = [];
    let sortedPlanets = [];

    const apiUrl =
        "https://api.le-systeme-solaire.net/rest/bodies?filter[]=bodyType,eq,planet&data=id,englishName,semimajorAxis,meanRadius,gravity,sideralOrbit";

    onMount(async () => {
        const res = await fetch(apiUrl);
        const data = await res.json();
        planets = data.bodies;
        sortPlanets();
    });

    function sortPlanets() {
        sortedPlanets = [...planets].sort((a, b) => {
            let aKey = a[$sortKey];
            let bKey = b[$sortKey];
            if (aKey < bKey) return $sortDirection === "asc" ? -1 : 1;
            if (aKey > bKey) return $sortDirection === "asc" ? 1 : -1;
            return 0;
        });
    }

    function handleSort(key) {
        console.log("handleSort", key, $sortKey, $sortDirection);
        if ($sortKey === key) {
            $sortDirection = $sortDirection === "asc" ? "desc" : "asc";
        } else {
            $sortKey = key;
            $sortDirection = "asc";
        }
        sortPlanets();
    }
</script>

<table>
    <thead>
        <tr>
            <th on:click={() => handleSort("englishName")}>Name</th>
            <th on:click={() => handleSort("semimajorAxis")}>
                Semi-Major Axis (km)
            </th>
            <th on:click={() => handleSort("meanRadius")}>Mean Radius (km)</th>
            <th on:click={() => handleSort("gravity")}>Gravity (m/s²)</th>
            <th on:click={() => handleSort("sideralOrbit")}
                >Orbital Period (days)</th
            >
        </tr>
    </thead>
    <tbody>
        {#each sortedPlanets as planet}
            <tr>
                <td>
                    <a href={`#/planet/${planet.id}`}>{planet.englishName}</a>
                </td>
                <td>{planet.semimajorAxis}</td>
                <td>{planet.meanRadius}</td>
                <td>{planet.gravity}</td>
                <td>{planet.sideralOrbit}</td>
            </tr>
        {/each}
    </tbody>
</table>

<style>
    table {
        width: 100%;
        max-width: 800px;
        border-collapse: collapse;
    }

    thead tr {
        background-color: #8f8f8f;
    }

    th,
    td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: right;
    }

    th {
        cursor: pointer;
    }

    th:hover {
        background-color: #f2f2f2;
    }

    tr:nth-child(even) {
        background-color: #f2f2f2;
    }
</style>
