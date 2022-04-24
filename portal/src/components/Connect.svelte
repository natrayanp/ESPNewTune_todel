<script>
  import { onMount, createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  
  export let selecteddt ={};
  let ssid;
  let direct_connect;  
  let orgpassword;  
  let loading = false;
  let password = "";
  let inlist = false;

  async function connect(){
    loading = true;
    let formData = new FormData();
    formData.append('ssid', ssid);
    formData.append('password', password);

    if(inlist) {
      if(direct_connect){
        formData.append('optyp', "CON");
      } else {
        if(password == orgpassword){
          formData.append('optyp', "CON");
      } else {
        formData.append('optyp', "UPD");
      }
      }
    } else {
      formData.append('optyp', "ADD");
    }
    
    
    const res = await fetch(`/espconnect/connect`, { method: 'POST', body: formData });
		if (res.status === 200) {
      dispatch('success');
		} else {      
      dispatch('error');
    }
    loading = false;
		return res;
  }


  onMount(async () => {

    ssid = selecteddt.ssid;
    direct_connect = selecteddt.direct_connect;
    password=selecteddt.password;
    orgpassword=selecteddt.password;
    inlist = selecteddt.inlist;


    if(direct_connect){
      loading = true;
      await connect();
      loading = false;
    }
  })
</script>
<!--
<form class="container d-flex flex-columns" on:submit|preventDefault={connect}>
  <div class="row h-100">
    <div class="column">
      <div class="row">
        <div class="column text-center">
          <p class="mb-2 text-muted">
            Connect to WiFi
          </p>
        </div>
      </div>
      <div class="row">
        <div class="column column-100">
          <input type="text" placeholder="SSID" id="ssid" value={ssid} disabled required>
        </div>
      </div>
      <div class="row">
        <div class="column column-100">
          <input type="password" placeholder="WiFi Password" id="password" bind:value={password} disabled={loading} required minlength="8">
        </div>
      </div>
    </div>
  </div>
  <div class="row flex-rows">
    <div class="column w-auto pr-1">
      <button type="button" class="button btn-light text-center" on:click={back} disabled={loading}>
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" style="fill: #fff; vertical-align: middle"><path d="M21 11L6.414 11 11.707 5.707 10.293 4.293 2.586 12 10.293 19.707 11.707 18.293 6.414 13 21 13z"></path></svg>
      </button>
    </div>
    <div class="column">
      <button type="submit" class="button w-100 text-center" disabled={loading}>
        {#if loading}
          <div class="btn-loader"></div>
        {:else}
          Connect
        {/if}
      </button>
    </div>
  </div>
</form>
-->


<form class="flex flex-col mb-2 min-w-full p-2 w-auto"  on:submit|preventDefault={connect}>      
  <div class="flex flex-row justify-center h-16 text-blue-800">
    <p class="text-2xl">Connect to WiFi</p>
  </div>           
  <div class="flex flex-row flex-grow h-16 mb-10">    
      <input 
      class = "text-black text-xl"
      type="text" placeholder="SSID" id="ssid" value={ssid} disabled required>
  </div>
  <div class="flex flex-row flex-grow  h-8 mb-10">
    <input 
    class="mt-0 block w-full border-0 border-b-2 border-gray-200 focus:ring-0 focus:ring-offset-0 focus:border-blue hover:border-blue hover:border-b"
    type="password" placeholder="WiFi Password" id="password" bind:value={password} disabled={loading} required minlength="8">
  </div>
  <div class="text-center mt-5">      
    <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full border-none" disabled={loading}>
      Connect
    </button>
  </div>
</form>

