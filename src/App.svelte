<script lang="ts">
  import { Datatable, rows } from 'svelte-simple-datatables';
  import type { Dog } from './models/dog.model';
  const settings = {
    sortable: true,
    pagination: true,
    rowPerPage: 50,
    scrollY: true,
  };

  const fetchDogBreedList: Promise<Dog[]> = (async () => {
    const response = await fetch('https://api.thedogapi.com/v1/breeds', {
      method: 'GET',
      headers: {
        'x-api-key': '707a54e0-1133-44cc-bb27-3a319c6a259c',
      },
    });
    return await response.json();
  })();
</script>

<main class="flex flex-col p-4 h-full w-full">
  {#await fetchDogBreedList}
    <p>...waiting</p>
  {:then data}
    <Datatable {settings} {data}>
      <thead>
        <th data-key="image">Image</th>
        <th data-key="name">Breed</th>
        <th data-key="bred_for">Bred For</th>
        <th data-key="weight.imperial">Weight (kg)</th>
        <th data-key="height.imperial">Height (cm)</th>
        <th data-key="life_span">Life Span</th>
        <th data-key="breed_group">Breed Group</th>
      </thead>
      <tbody>
        {#each $rows as row}
          <tr>
            <td class="flex flex-col items-center justify-center">
              <img class="object-cover w-20 h-20 rounded" src={row.image.url} alt="" />
            </td>

            <td>{row.name}</td>

            {#if row.bred_for}
              <td>{row.bred_for}</td>
            {:else}
              <td>{''}</td>
            {/if}

            <td>{row.weight.imperial}</td>
            <td>{row.height.imperial}</td>
            <td>{row.life_span}</td>

            {#if row.breed_group}
              <td>{row.breed_group}</td>
            {:else}
              <td>{'Uncategorized'}</td>
            {/if}
          </tr>
        {/each}
      </tbody>
    </Datatable>
    <!-- <pre>{JSON.stringify( data, null, 2)}</pre> -->
  {:catch error}
    <p>An error occurred!</p>
  {/await}
</main>

<style global lang="postcss">
  :global(body) {
    padding: 0;
    margin: 0;
  }

  @tailwind base;
  @tailwind components;

  @tailwind utilities;
</style>
