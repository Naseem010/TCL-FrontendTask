<script lang="ts">
  import { onMount } from "svelte";
  import type { Contact } from "./contacts.interface.ts";
  import ContactInfo from "./contactInfo.svelte";
  import vector from "./image/Vector.png";
  import { createEventDispatcher } from "svelte";
  // export let contacts: Contact[] = [];
  export let contacts;
  export let originalContacts;
  export let filteredContacts;
  export let searchQuery;
  // let contacts: Contact[] = [];
  let dispatch = createEventDispatcher();

  export let flag: boolean = false;
  function handleClick(contact: Contact) {
    flag = !flag;
    dispatch("contactSelected", contact);
  }
  async function fetchContacts() {
    try {
      const response = await fetch("https://dummyjson.com/users");
      const data = await response.json();
      contacts = data.users;
      //   console.log(contacts);
    } catch (error) {
      console.error("Error fetching contacts:", error);
    }
  }

  function groupContactsByAlphabet() {
    const groupedContacts: Record<string, Contact[]> = {};

    contacts.forEach((contact) => {
      const firstLetter = contact.firstName.charAt(0).toUpperCase();
      if (!groupedContacts[firstLetter]) {
        groupedContacts[firstLetter] = [];
      }
      groupedContacts[firstLetter].push(contact);
    });
    // console.log(groupedContacts);
    return groupedContacts;
  }

  let groupedContacts: Record<string, Contact[]> = {};

  onMount(async () => {
    await fetchContacts();
    groupedContacts = groupContactsByAlphabet();
    // console.log('filteredContacts:', filteredContacts);
    // console.log(groupedContacts);
  });
</script>

<div>
  {#each Object.keys(groupedContacts).sort() as letter}
    <div class="relative">
      <div
        class="py-5 [font-family:'Nunito-Bold',Helvetica] font-bold text-black text-[14px]"
      >
        <h2 class="absolute right-[17px]">{letter}</h2>
      </div>
      <div class="flex flex-col px-6">
        {#each groupedContacts[letter] as contact}
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <!-- svelte-ignore a11y-no-static-element-interactions -->
          <div
            class="flex relative py-2 border-b-[1px] border-[#B6B6B6]"
            on:click={() => handleClick(contact)}
          >
            <div class="w-[41px] h-[41px]">
              <img class="object-cover" src={contact.image} alt="Profile Pic" />
            </div>
            <div class="mt-[10px]">
              <p
                class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black text-[13px]"
              >
                {contact.firstName}{" "}{contact.lastName}
              </p>
              <p
                class="[font-family:'Nunito-Regular',Helvetica] font-normal text-[#b6b6b6] text-[12px]"
              >
                {contact.phone}
              </p>
            </div>
            <div class="w-[23px] h-[6px] absolute right-0 top-1/2">
              <img src={vector} class=" w-[23px] h-[6px]" alt="Vector" />
            </div>
          </div>
        {/each}
      </div>
    </div>
  {/each}
</div>

<!-- <div>
  {#if Object.keys(filteredContacts).length > 0}
    {#each Object.keys(filteredContacts).sort() as letter}
      <div class="relative">
        <div
          class="py-5 [font-family:'Nunito-Bold',Helvetica] font-bold text-black text-[14px]"
        >
          <h2 class="absolute right-6">{letter}</h2>
        </div>
        <div class="flex flex-col">
          {#each groupedContacts[letter] as contact}
            <div class="flex relative px-3 py-2">
              <div class="w-[41px] h-[41px]">
                <img class="object-cover" src={contact.image} alt="Profile Pic" />
              </div>
              <div class="mt-[10px]">
                <p
                  class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black text-[13px]"
                >
                  {contact.firstName}{" "}{contact.lastName}
                </p>
                <p
                  class="[font-family:'Nunito-Regular',Helvetica] font-normal text-[#b6b6b6] text-[12px]"
                >
                  {contact.phone}
                </p>
              </div>
              <div class="w-[23px] h-[6px] absolute right-10 top-1/2">
                <img src={vector} class=" w-[23px] h-[6px]" alt="Vector" />
              </div>
            </div>
            <div class="w-5/6 ml-8 px-10 my-1 h-[1px] bg-slate-400" />
          {/each}
        </div>
      </div>
    {/each}
  {:else}
    <p>No contacts found.</p>
  {/if}
</div> -->
