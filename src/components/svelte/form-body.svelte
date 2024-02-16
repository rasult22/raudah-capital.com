<script>
import PocketBase from 'pocketbase';
const pb = new PocketBase('https://rasult22.pockethost.io');
let isDone = false
let isLoading = false
let showError = false
let fileInput = null
let name = ''
let company = ''
let email = ''
let phone = ''
let project_state = ''
let project_goals = ''

const onSubmit = (e) => {
  if (!fileInput.files.length || !name || !company || !email || !phone || !project_goals || !project_state) {
    showError = true
    e.preventDefault()
    return
  }
  showError = false
  if (fileInput.files.length) {
    isLoading = true
    const formData = new FormData();
    formData.append('email', email)
    formData.append('phone', phone)
    formData.append('person_name', name)
    formData.append('company_name', company)
    formData.append('project_state', project_state)
    formData.append('project_goals', project_goals)
    formData.append('project_pdf', fileInput.files[0])
    pb.collection('startups').create(formData).then(() => {
      isDone = true
    }).finally(() => {
      isLoading = false
    })
  }
  e.preventDefault()
}
</script>

<form action="" method="post" on:submit={onSubmit} class="mx-auto max-w-[560px] mt-[20px] text-[16px] relative">
  {#if isDone}
  <div class="text-center w-full text-[20px] font-semibold absolute z-10 top-[50%] -translate-y-[100%]">
    Заявка успешно отправлена ✅
  </div>
  {/if}
  {#if isLoading}
  <div class="flex justify-center w-full text-[20px] font-semibold absolute z-10 top-[50%] -translate-y-[100%]">
    <div role="status">
        <svg aria-hidden="true" class="w-8 h-8 text-blue-100 animate-spin fill-[#0080B5]" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
            <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
        </svg>
        <span class="sr-only">Loading...</span>
    </div>
  </div>
  {/if}
  <div class={isDone || isLoading ? 'opacity-[0.4]' : ''}>
    <div class="flex sm:flex-wrap sm:space-x-0 sm:space-y-[14px] space-x-2 relative">
      <input
        bind:value={name}
        class="border border-[#E0E3EB] w-full py-[10px] px-[8px] rounded-[4px]"
        type="text"
        placeholder="Ваше имя*"
      />
      <span class="text-red-500 text-[25px] absolute -left-1 sm:-top-4 sm:left-1 -top-1">*</span>
      <input
        bind:value={company}
        class="border border-[#E0E3EB] w-full py-[10px] px-[8px] rounded-[4px]"
        type="text"
        placeholder="Ваша компания*"
      />
      <span class="text-red-500 text-[25px] absolute sm:left-1 sm:top-[40%] left-[50%] -top-1">*</span>
    </div>
    <div class="flex sm:flex-wrap sm:space-x-0 sm:space-y-[14px] space-x-2 mt-[14px] relative">
      <input
        bind:value={email}
        class="relative border border-[#E0E3EB] w-full py-[10px] px-[8px] rounded-[4px]"
        type="email"
        placeholder="Ваш email*"
      />
      <span class="text-red-500 text-[25px] absolute -left-1 sm:-top-4 sm:left-1 -top-1">*</span>
      <input
        bind:value={phone}
        class="border border-[#E0E3EB] w-full py-[10px] px-[8px] rounded-[4px]"
        type="text"
        placeholder="Ваш номер*"
      />
      <span class="text-red-500 text-[25px] absolute sm:left-1 sm:top-[40%] left-[50%] -top-1">*</span>
    </div>
    <div class="relative">
      <span class="text-red-500 text-[25px] absolute left-1 top-2">*</span>
      <input
        bind:value={project_state}
        class="mt-[14px] border w-full border-[#E0E3EB] py-[10px] px-[8px] rounded-[4px]"
        type="text"
        placeholder="На какой стадии ваш проект?*"
      />
    </div>
    <div class="relative">
      <span class="text-red-500 text-[25px] absolute left-1 top-2">*</span>
      <textarea
        bind:value={project_goals}
        class="mt-[14px] border w-full border-[#E0E3EB] py-[10px] px-[8px] rounded-[4px]"
        type="text"
        placeholder="О цели вашего проекта (коротко)*"
      />
    </div>
    <div
      class="flex flex-wrap relative space-x-1 border bg-white border-[#E0E3EB] py-[11px] px-[12px] rounded-[4px]"
    >
      <span class="text-red-500 text-[25px] absolute left-1 top-0">*</span>
      <svg
        width="24"
        height="25"
        viewBox="0 0 24 25"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M18.08 12.7295L11.9 18.9195C11.0898 19.6396 10.0352 20.0228 8.95173 19.9909C7.86831 19.959 6.83801 19.5144 6.07158 18.748C5.30515 17.9815 4.86052 16.9512 4.82862 15.8678C4.79673 14.7844 5.17999 13.7297 5.90001 12.9195L13.9 4.91954C14.3776 4.46584 15.0112 4.21287 15.67 4.21287C16.3288 4.21287 16.9624 4.46584 17.44 4.91954C17.9054 5.39113 18.1663 6.02701 18.1663 6.68954C18.1663 7.35207 17.9054 7.98795 17.44 8.45954L10.54 15.3495C10.4717 15.4231 10.3896 15.4824 10.2984 15.5243C10.2072 15.5661 10.1086 15.5895 10.0083 15.5932C9.908 15.5969 9.80797 15.5809 9.71389 15.5459C9.61982 15.511 9.53355 15.4578 9.46001 15.3895C9.38647 15.3213 9.3271 15.2391 9.28529 15.1479C9.24348 15.0567 9.22005 14.9581 9.21633 14.8578C9.21262 14.7575 9.22869 14.6575 9.26364 14.5634C9.29859 14.4693 9.35172 14.3831 9.42001 14.3095L14.55 9.18954C14.7383 9.00124 14.8441 8.74584 14.8441 8.47954C14.8441 8.21324 14.7383 7.95784 14.55 7.76954C14.3617 7.58123 14.1063 7.47545 13.84 7.47545C13.5737 7.47545 13.3183 7.58123 13.13 7.76954L8.00001 12.9095C7.74331 13.1642 7.53957 13.4672 7.40054 13.8011C7.2615 14.1349 7.18992 14.4929 7.18992 14.8545C7.18992 15.2162 7.2615 15.5742 7.40054 15.908C7.53957 16.2418 7.74331 16.5448 8.00001 16.7995C8.52438 17.299 9.22081 17.5776 9.94501 17.5776C10.6692 17.5776 11.3656 17.299 11.89 16.7995L18.78 9.89954C19.5749 9.04649 20.0076 7.91821 19.9871 6.7524C19.9665 5.5866 19.4942 4.47429 18.6697 3.64981C17.8453 2.82533 16.7329 2.35306 15.5671 2.33249C14.4013 2.31192 13.2731 2.74466 12.42 3.53954L4.42001 11.5395C3.34121 12.7344 2.76504 14.2994 2.81154 15.9085C2.85805 17.5176 3.52364 19.0468 4.66965 20.1773C5.81565 21.3078 7.35368 21.9526 8.96329 21.9772C10.5729 22.0019 12.1299 21.4045 13.31 20.3095L19.5 14.1295C19.5932 14.0363 19.6672 13.9256 19.7177 13.8038C19.7681 13.682 19.7941 13.5514 19.7941 13.4195C19.7941 13.2877 19.7681 13.1571 19.7177 13.0353C19.6672 12.9135 19.5932 12.8028 19.5 12.7095C19.4068 12.6163 19.2961 12.5423 19.1743 12.4919C19.0524 12.4414 18.9219 12.4154 18.79 12.4154C18.6581 12.4154 18.5276 12.4414 18.4058 12.4919C18.2839 12.5423 18.1732 12.6163 18.08 12.7095V12.7295Z"
          fill="black"
        />
      </svg>
      <span class="font-bold text-[#0080B5]">Прикрепить </span>
      <span class="text-[#A3AAC2]"
        >презентацию с бизнес планом о проекте (в PDF)*</span
      >
      <input bind:this={fileInput} class="block w-full mt-[14px]" accept=".pdf" type="file" />
    </div>
    <div>
      <span class="text-red-500 text-[25px]">*</span>
      обязательные поля
    </div>
    {#if showError}
      <div class="mt-[10px] text-center">
        <div class="flex items-center p-4 mb-4 text-sm sm:text-[14px] text-red-800 border border-red-300 rounded-lg bg-red-50 " role="alert">
          <svg class="flex-shrink-0 inline w-4 h-4 me-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 20 20">
            <path d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z"/>
          </svg>
          <span class="sr-only">Info</span>
          <div>
            Заполните все обязательные поля!
          </div>
        </div>
      </div>
    {/if}
    <div class="flex justify-center mt-[41px]">
      <button
        type="submit"
        class="disabled:cursor-not-allowed disabled:opacity-[0.6] sm:disabled:opacity-[0.2] text-[16px] font-medium rounded-[8px] py-[15px] px-[44px] font-railway hover:opacity-[0.8] active:opacity-[0.7] bg-[#0080B5] text-white"
      >
        Подать заявку
      </button>
    </div>
  </div>
</form>
