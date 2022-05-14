<script>
  import "./tailwind.css";
  import "flowbite/dist/flowbite.css";
  let username = "";
  let password = "";
  let user = {};
  let firstCall = true;
  user.authorized = false;

  export async function get() {
    console.log("GET IS CALLED");
    if (!firstCall && user.authorized) {
      return {
        status: 302,
        headers: { Location: "/main" },
      };
    } else {
      return {
        status: 302,
        headers: { Location: "/" },
      };
    }
  }

  function doLogin() {
    console.log("Processing login for " + username);
    fetch("http://127.0.0.1:3000/api/v2/users")
      .then((r) => {
        r.json().then((jd) => {
          if (jd.code === "ERROR") {
            firstCall = false;
            user.authorized = false;
          } else {
            firstCall = false;
            user.authorized = true;
          }
        });
      })
      .catch((error) => {
        console.error(error);
      });
  }
</script>

<div class="grid md:grid-cols-3 mt-6">
  <div />
  <div class=" max-w-sm border border-grey-200 rounded-lg p-5 shadow-lg">
    <div>
      <div>
        <label
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
          for="username">Username</label
        >
        <input
          bind:value={username}
          id="username"
          class="data-lpignore bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          type="text"
        />
      </div>
      <div class="mt-3">
        <label
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
          for="password">Password</label
        >
        <input
          id="password"
          bind:value={password}
          class="data-lpignore bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          type="password"
        />
      </div>
      <div class="flex">
        <button
          on:click={doLogin}
          class="mb-1 text-sm font-bold mt-3 p-2 rounded-lg bg-blue-100"
          >Login</button
        >
        {#if !firstCall && !user.authorized}
          <div class="text-red-500 font p-4 font-bold font-medium">
            Unauthorized
          </div>
        {/if}
      </div>
    </div>
  </div>
</div>
