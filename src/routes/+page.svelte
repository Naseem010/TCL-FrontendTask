<script lang="ts">
  import ContactDetails from "./contactDetails.svelte";
  import { onMount } from "svelte";
  import type { Contact } from "./contacts.interface.ts";
  import ContactInfo from "./contactInfo.svelte";
  let flag:boolean;
  let contacts: Contact[] = [];
  let originalContacts: Contact[] = [];
  let searchQuery = '';
  let filteredContacts: Record<string, Contact[]> = {};
  async function fetchContacts() {
    try {
      const response = await fetch("https://dummyjson.com/users");
      const data = await response.json();
      contacts = data.users;
      originalContacts = [...contacts]; // Store original contacts for filtering
    } catch (error) {
      console.error("Error fetching contacts:", error);
    }
  }

 
  onMount(async () => {
    await fetchContacts();
  });
 
  $: {
    filteredContacts = filterContacts();
  }

  function filterContacts() {
    if (!searchQuery || searchQuery.trim() === "") {
      return groupContactsByAlphabet();
    }

    const filtered: Record<string, Contact[]> = {};
    const searchTerm = searchQuery.toLowerCase().trim();
    originalContacts.forEach((contact) => {
      if (
        contact.firstName.toLowerCase().includes(searchTerm) ||
        contact.lastName.toLowerCase().includes(searchTerm) ||
        contact.phone.includes(searchTerm)
      ) {
        const firstLetter = contact.firstName.charAt(0).toUpperCase();
        if (!filtered[firstLetter]) {
          filtered[firstLetter] = [];
        }
        filtered[firstLetter].push(contact);
      }
    });
   
    return filtered;
  }
  // console.log('filteredContacts in +page.svelte:', filteredContacts);

  function groupContactsByAlphabet() {
    const grouped: Record<string, Contact[]> = {};

    contacts.forEach((contact) => {
      const firstLetter = contact.firstName.charAt(0).toUpperCase();
      if (!grouped[firstLetter]) {
        grouped[firstLetter] = [];
      }
      grouped[firstLetter].push(contact);
    });

    return grouped;
  }
  let selectedContact:Contact[]=[];
  function handleContactSelected(event) {
    selectedContact = event.detail;
  }
  // $:console.log(selectedContact);
  
</script>

<svelte:head>
  <title>My Contacts</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>
  {#if flag}
  <section class="md:w-1/3 w-4/5 max-h-screen overflow-scroll bg-white rounded-lg">
	<ContactInfo bind:flag {selectedContact}  />
</section>
{:else}
<section class="md:w-1/3 w-4/5 max-h-screen overflow-scroll bg-white rounded-lg">
<div
class="px-6 pt-10 pb-3 [font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black text-[24px] tracking-[0] leading-[normal]"
>
<h1>My Contacts</h1>
</div>
<div class="px-5 pb-3 relative ">
<input
  class="w-full border-2 h-10 px-5 rounded-[12px] text-sm focus:outline-none pl-10 shadow-[0px_1px_12px_1px_#00000040] [font-family:'Nunito-Regular',Helvetica]"
  type="search"
  name="search"
  placeholder="Search by name or number"
  bind:value={searchQuery}
/>
<span class="absolute left-9 top-[10px] text-[#B7B7B7]">
  <svg class=" h-5 w-4 fill-current" viewBox="0 0 56.966 56.966">
    <path
      d="M55.146,51.887L41.588,37.786c3.486-4.144,5.396-9.358,5.396-14.786c0-12.682-10.318-23-23-23s-23,10.318-23,23  s10.318,23,23,23c4.761,0,9.298-1.436,13.177-4.162l13.661,14.208c0.571,0.593,1.339,0.92,2.162,0.92  c0.779,0,1.518-0.297,2.079-0.837C56.255,54.982,56.293,53.08,55.146,51.887z M23.984,6c9.374,0,17,7.626,17,17s-7.626,17-17,17  s-17-7.626-17-17S14.61,6,23.984,6z"
    />
  </svg>
</span>
</div>
<div
class=" px-5 pt-4 [font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black text-[14px]"
>
Contacts
</div>
<ContactDetails bind:flag on:contactSelected={handleContactSelected} contacts={contacts} originalContacts={originalContacts} filteredContacts={filteredContacts} searchQuery={searchQuery}/>
</section>
{/if}