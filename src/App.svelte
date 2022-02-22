<script lang="ts">
  import { Command } from "@tauri-apps/api/shell";

  type Edition = {
    longname: string;
    shortname: string;
    key: string;
  };

  let version = 11;
  let isActivating: boolean = false;
  let selectedEdition = "";

  const activateWindows = async () => {
    isActivating = true;
    const edition = editions.find(
      (edition) => edition.shortname == selectedEdition
    );

    const setLicenseKey = new Command("echo-test", [edition.key]);
    setLicenseKey.on("error", (error) =>
      console.error(`command error: "${error}"`)
    );
    setLicenseKey.stdout.on("data", (line) =>
      console.log(`command stdout: "${line}"`)
    );
    setLicenseKey.stderr.on("data", (line) =>
      console.log(`command stderr: "${line}"`)
    );

    const child = await setLicenseKey.spawn();
  };

  const editions: Edition[] = [
    {
      longname: `Windows ${version} Home`,
      shortname: "home",
      key: "TX9XD-98N7V-6WMQ6-BX7FG-H8Q99",
    },
    {
      longname: `Windows ${version} Pro`,
      shortname: "pro",
      key: "W269N-WFGWX-YVC9B-4J6C9-T83GX",
    },
    {
      longname: `Windows ${version} Education`,
      shortname: "edu",
      key: "NW6C2-QMPVW-D7KKK-3GKT6-VCFB2",
    },
    {
      longname: `Windows ${version} Enterprise`,
      shortname: "ent",
      key: "NPPR9-FWDCX-D2C8J-H872K-2YT43",
    },
  ];
</script>

<main
  class="flex items-center justify-center w-full h-screen flex-col font-sans"
>
  <div class="flex flex-col justify-center items-center">
    <img src="/windows_logo.svg" alt="Windows Logo" />
    <h1 class="font-bold text-3xl mt-2">Windows Activator</h1>
    <form
      class="mt-4 flex flex-col items-center w-full"
      on:submit|preventDefault={activateWindows}
    >
      <label for="edition" class="w-full text-left">Edition:</label>
      <select
        name="edition"
        class=" w-full border border-gray-400 bg-transparent"
        bind:value={selectedEdition}
      >
        {#each editions as edition}
          <option value={edition.shortname}>{edition.longname}</option>
        {/each}
      </select>
      <button type="submit" class="mt-2 w-3/4 border rounded-md"
        >Activate</button
      >
      {#if isActivating}
        <div />
      {/if}
    </form>
  </div>
</main>
