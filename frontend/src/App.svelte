<script>
  import { onMount } from 'svelte';
  import axios from 'axios';

  let message = 'Loading...';
  let apiStatus = 'Checking API...';
  let users = [];

  onMount(async () => {
    try {
      // Test API connection
      const response = await axios.get('/api/health');
      apiStatus = '✅ API is healthy';
      
      // Get users
      const usersResponse = await axios.get('/api/users');
      users = usersResponse.data.users;
      message = '✅ Connected to backend!';
    } catch (error) {
      console.error('Error:', error);
      message = '❌ Error connecting to backend';
      apiStatus = '❌ API not reachable';
    }
  });

  let name = '';
  let submitted = false;

  function handleSubmit() {
    if (name.trim()) {
      users.push(name);
      name = '';
      submitted = true;
      setTimeout(() => submitted = false, 2000);
    }
  }
</script>

<main>
  <div class="container">
    <h1>🚀 Svelte + Express + PostgreSQL</h1>
    <h2>Deployed on Azure!</h2>
    
    <div class="status-card">
      <h3>📊 Status</h3>
      <p><strong>API Status:</strong> {apiStatus}</p>
      <p><strong>Message:</strong> {message}</p>
    </div>

    <div class="users-card">
      <h3>👥 Users List</h3>
      {#if users.length > 0}
        <ul>
          {#each users as user}
            <li>{user}</li>
          {/each}
        </ul>
      {:else}
        <p>No users yet</p>
      {/if}
    </div>

    <div class="add-user">
      <h3>➕ Add New User</h3>
      <form on:submit|preventDefault={handleSubmit}>
        <input 
          type="text" 
          bind:value={name} 
          placeholder="Enter name..."
          required
        />
        <button type="submit">Add User</button>
      </form>
      {#if submitted}
        <p class="success">✅ User added successfully!</p>
      {/if}
    </div>

    <div class="footer">
      <p>Built with ❤️ using Svelte, Express, and PostgreSQL on Azure</p>
      <p><small>Azure Database for PostgreSQL Flexible Server</small></p>
    </div>
  </div>

  <style>
    .container {
      max-width: 600px;
      margin: 50px auto;
      padding: 20px;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
    }

    h1 {
      color: #0078d4;
      text-align: center;
    }

    h2 {
      color: #666;
      text-align: center;
      font-weight: normal;
    }

    .status-card, .users-card, .add-user {
      background: #f5f5f5;
      padding: 20px;
      border-radius: 8px;
      margin: 20px 0;
      border: 1px solid #e0e0e0;
    }

    .status-card h3, .users-card h3, .add-user h3 {
      margin-top: 0;
      color: #333;
    }

    .users-card ul {
      list-style: none;
      padding: 0;
    }

    .users-card li {
      padding: 8px 12px;
      background: white;
      margin: 5px 0;
      border-radius: 4px;
      border-left: 3px solid #0078d4;
    }

    .add-user input {
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 4px;
      width: 70%;
      margin-right: 10px;
    }

    .add-user button {
      padding: 10px 20px;
      font-size: 16px;
      background: #0078d4;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    .add-user button:hover {
      background: #005a9e;
    }

    .success {
      color: #00a300;
      font-weight: bold;
      margin-top: 10px;
    }

    .footer {
      text-align: center;
      margin-top: 30px;
      color: #888;
      border-top: 1px solid #e0e0e0;
      padding-top: 20px;
    }

    .footer small {
      color: #aaa;
    }
  </style>
</main>
