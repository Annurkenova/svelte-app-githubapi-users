<script>
    let username = "";
    let userData = null;
  
    const apiUrl = "https://api.github.com/users/";
  
    const searchUser = async () => {
      try {
        const response = await fetch(apiUrl + username, {
          headers: {
            Authorization: `token ${import.meta.env.VITE_GITHUB_TOKEN}`
          }
        });
  
        if (response.ok) {
          userData = await response.json();
        } else {
          console.error(`Error: ${response.status} - ${response.statusText}`);
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };
  </script>
  <main>
    <img src="/devfinder.svg">
    <input bind:value={username} placeholder="Enter GitHub username" />
    <button on:click={searchUser}>Search</button>
  
    {#if userData}
      <div>
        <img src={userData.avatar_url} class="user" alt="User Avatar" />
        <h2>{userData.login}</h2>
        <h2>{userData.name || userData.login}</h2>
        <p>Public Repos: {userData.public_repos}</p>
        <p>Followers: {userData.followers}</p>
        <p>Following: {userData.following}</p>
        <p>Registration Date: {new Date(userData.created_at).toLocaleDateString()}</p>
        <p>Location: {userData.location || "Not specified"}</p>
        <p>GitHub Profile: <a href={userData.html_url} target="_blank">{userData.html_url}</a></p>
        <p>Twitter: {userData.twitter_username || "Not specified"}</p>
        <p>Company: {userData.company || "Not specified"}</p>
      </div>
    {/if}
  </main>
  
  <style>
    main {
      text-align: center;
      margin-top: 2rem;
    }
  
    label {
      margin-right: 0.5rem;
    }
  
    input {
      margin-right: 1rem;
    }
  
    button {
      cursor: pointer;
    }
  
    .user {
      border-radius: 50%;
      width: 100px;
      height: 100px;
      object-fit: cover;
    }
  </style>