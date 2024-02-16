<script>
let items = [
  {
    id: 1,
    title: 'Какие сферы мы финансируем?',
    isOpen: true,
    content: 'Особое внимание уделяется стартапам в таких сферах, как образование, здравоохранение, финансовые технологии, устойчивое развитие и социальное предпринимательство, где инновации могут сыграть ключевую роль в улучшении качества жизни.',
  },
  {
    id: 2,
    title: 'Сумма финансирования',
    isOpen: false,
    content: 'content',
  },
  {
    id: 3,
    title: 'Какие критерия есть для заявки?',
    isOpen: false,
    content: 'content',
  },
]
let prevId = 1

const open = (id) => {
  if (id === prevId) return

  const item = items.find(x => x.id === prevId)
  item.isOpen = false

  const obj = items.find(x => x.id === id)
  if (obj) {
    prevId = obj.id
    obj.isOpen = true
  }
  items = [...items]
}
</script>

{#each items as item (item.id) }
  <div class="relative mb-3 bg-[#edf7ff] rounded-[8px] font-railway">
    <h6 class="mb-0">
      <button
        on:click={() => open(item.id)}
        class="relative text-[19px] sm:text-[16px] flex items-center justify-between w-full p-4 text-left transition-all ease-in border-b border-solid cursor-pointer border-slate-100 text-slate-700 rounded-t-1 group text-dark-500"
        data-collapse-target="collapse-1"
      >
        <span>{item.title}</span>
        <svg class={item.isOpen ? 'rotate-180' : ''} width="25" height="25" viewBox="0 0 25 25" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M20.8147 9.60089L14.2947 16.1209C13.5247 16.8909 12.2647 16.8909 11.4947 16.1209L4.9747 9.60089" stroke="#0080B5" stroke-width="1.5" stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </h6>
    <div
      data-collapse="collapse-1"
      class={`${item.isOpen ? 'h-[95px] sm:h-[200px] overflow-visible' : 'h-0  overflow-hidden'}  transition-all duration-300 ease-in-out`}
    >
      <div class="p-4 text-[15px] leading-normal text-blue-gray-500/80">
       {item.content}
      </div>
    </div>
  </div>
{/each}
