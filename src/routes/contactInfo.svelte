<script lang="ts">
  import arrow from "./image/arrow.png";
  import mIcon from "./image/mIcon.svg";
  import Vector from "./image/VectorVertical.png";
  import mailIcon from "./image/mailIcon.svg";
  import pIcon from "./image/pIcon.png";
  import wIcon from "./image/wIcon.svg";
  import teleIcon from "./image/teleIcon.svg";
  import type { Contact } from "./contacts.interface";

  export let flag: boolean = true;
  export let selectedContact;
  //  console.log(selectedContact);

  function handleClick() {
    flag = !flag;
  }
</script>

<svelte:head>
  <title>Contacts Info</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>
<div class="main">
  <div class="Top px-6 py-6 flex flex-col border-b-2 border-[#B7B7B7]">
    <div class="top-header flex justify-between items-center">
      <div>
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <img
          class=" w-[13px] h-[23px] hover:scale-125 hover:cursor-pointer"
          alt="Vector"
          src={arrow}
          on:click={handleClick}
        />
      </div>
      <div
        class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[36px] text-[28px]"
      >
        <h1>Contacts</h1>
      </div>
      <div>
        <img class="w-[7px] h-[27px]" alt="Vector" src={Vector} />
      </div>
    </div>
    <div class="flex justify-center my-2">
      <img
        class=" w-[100px] h-[100px] md:w-[138px] md:h-[138px] rounded-full"
        alt="profile"
        src={selectedContact.image}
      />
    </div>
    <div class=" text-center">
      <p
        class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black text-[20px]"
      >
        {selectedContact.firstName}{" "}{selectedContact.lastName}
      </p>
      <p
        class="[font-family:'Nunito-Regular',Helvetica] font-normal text-black text-[16px]"
      >
        {selectedContact.address.city}, {selectedContact.address.state}
      </p>
    </div>
  </div>
  <div class="bg-[#9aadbe33]">
    <div class="flex justify-between pl-12 pr-6 py-3">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          Mobile
        </p>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-[#b6b6b6] md:text-[16px] text-[13px]"
        >
          {selectedContact.phone}
        </p>
      </div>
      <div class="flex">
        <a href="sms:{selectedContact.phone}"
          ><img class="" alt="Group" src={mIcon} /></a
        >
        <a href="tel:{selectedContact.phone}"
          ><img class="" alt="Group" src={pIcon} /></a
        >
      </div>
    </div>
    <div class="flex justify-between pl-12 pr-6">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          Email
        </p>
        <a
          href="mailto:{selectedContact.email}"
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-[#b6b6b6] md:text-[16px] text-[13px]"
          >{selectedContact.email}</a
        >
      </div>
      <div class="flex">
        <a href="mailto:{selectedContact.email}">
          <img class="" alt="Group" src={mailIcon} /></a
        >
      </div>
    </div>
    <div class="flex justify-between pl-12 pr-6 py-3">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          Groups
        </p>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-[#b6b6b6] md:text-[16px] text-[13px]"
        >
          UI Groups
        </p>
      </div>
    </div>
    <div
      class="flex justify-between pl-12 pr-6 py-3 bg-white border-b-2 border-[#B7B7B7]"
    >
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          Account Linked
        </p>
      </div>
    </div>
    <div class="flex justify-between pl-12 pr-6 py-3">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          Telegram
        </p>
      </div>
      <div class="flex">
        <a href="https://t.me/share/url?text=&phone={selectedContact.phone}">
          <img class="" alt="Group" src={teleIcon} /></a
        >
      </div>
    </div>
    <div class="flex justify-between pl-12 pr-6 py-3">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          WhatsApp
        </p>
      </div>
      <div class="flex">
        <a href="https://wa.me/{selectedContact.phone}">
          <img class="" alt="Group" src={wIcon} /></a
        >
      </div>
    </div>
    <div
      class="flex justify-between pl-12 pr-6 py-3 bg-white border-b-2 border-[#B7B7B7]"
    >
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          More Options
        </p>
      </div>
    </div>
    <div class="flex justify-between pl-12 pr-6 py-3">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          Share Contact
        </p>
      </div>
    </div>
    <div class="flex justify-between pl-12 pr-6 pt-3 pb-8">
      <div>
        <p
          class="[font-family:'Nunito-SemiBold',Helvetica] font-semibold text-black md:text-[20px] text-[17px]"
        >
          QR Code
        </p>
      </div>
    </div>
  </div>
</div>
