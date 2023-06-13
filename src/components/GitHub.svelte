<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import { Table, Button, Form, FormGroup, Input, Spinner } from "sveltestrap";

  let users = [];
  let searchTerm = "";
  let isLoading = false;

  const getData = async () => {
    const response = await axios.get(
      `https://api.github.com/search/users?q=${searchTerm}`
    );
    users = response.data.items;
    isLoading = false;
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    isLoading = true;
    getData();
  };

  onMount(getData);
</script>

<Form on:submit={handleSubmit}>
  <FormGroup floating label="Search">
    <Input
      type="text"
      bind:value={searchTerm}
      placeholder="Search for a GitHub user"
    />
  </FormGroup>
  <Button type="submit" color="primary">Submit</Button>
</Form>

{#if isLoading}
  <Spinner color="danger" />
{/if}

<Table>
  <thead>
    <tr>
      <th>ID</th>
      <th>Avatar</th>
      <th>Login</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    {#each users as user}
      <tr>
        <th scope="row">{user.id}</th>
        <td
          ><img
            src={user.avatar_url}
            alt={user.login}
            width="75"
            height="75"
          /></td
        >
        <td>{user.login}</td>
        <td><a href={user.html_url}>{user.html_url}</a></td>
      </tr>
    {/each}
  </tbody>
</Table>
