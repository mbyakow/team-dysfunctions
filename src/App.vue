<template>
  <div id="app" class="h-screen">
    <div class="lg:flex p-4 lg:p-0 block lg:w-2/3 mx-auto h-full justify-center" v-if="step === 0">
      <div class="self-center text-center flex-shrink-0 mb-4 lg:mb-0">
        <img class="mx-auto lg:mx-0" src="./assets/team.svg"/>
      </div>
      <div class="lg:text-left text-center leading-tight self-center lg:ml-16">
        <div class="font-bold lg:text-4xl text-3xl leading-none">Ответь на 15 вопросов и узнай, каким порокам подвержена твоя команда</div>
        <div class="text-xl mt-4">Сделано на основе книги <a class="underline text-blue-500" href="http://www.mann-ivanov-ferber.ru/books/biznesroman/arshipfable/" target="_blank">«Пять пороков команды»</a> Патрика Ленсиони</div>
        <div v-on:click="step++" class="mt-6 rounded-lg text-center text-white inline-block text-2xl py-2 px-4 font-bold cursor-pointer" style="background-color: #00a8ff;">Пройти тест</div>
      </div>
    </div>
    <div class="lg:text-xl text-lg h-screen flex" v-if="step > 0 && step <= 15">
      <div class="self-center text-center lg:w-1/2 p-4 lg:p-0 mx-auto">
        <div class="font-bold mb-2 text-2xl">Вопрос {{ step }} из 15</div>
        <div>{{ questions[step - 1].title }}</div>
        <div class="lg:flex block text-center justify-center">
          <div v-on:click="answer(3)" class="no-select lg:mt-6 mt-6 lg:mr-3 rounded-lg bg-blue-500 text-center text-white block py-2 px-4 font-bold cursor-pointer" style="background-color: #00a8ff;">Всегда</div>
          <div v-on:click="answer(2)" class="no-select lg:mt-6 mt-2 lg:mr-3 rounded-lg bg-blue-500 text-center text-white block py-2 px-4 font-bold cursor-pointer" style="background-color: #00a8ff;">Иногда</div>
          <div v-on:click="answer(1)" class="no-select lg:mt-6 mt-2 rounded-lg bg-blue-500 text-center text-white block py-2 px-4 font-bold cursor-pointer" style="background-color: #00a8ff;">Редко</div>
        </div>
      </div>
    </div>
    <div class="text-xl flex" v-if="step === 16">
      <div class="self-center text-left lg:w-1/2 w-screen break-words p-6 lg:p-0 mx-auto mb-12">
        <div class="intro lg:mt-6 mb-4 font-bold">Итоги</div>

        <div class="leading-tight">
          <div class="lg:text-4xl text-2xl font-bold">Недоверие</div>
          <div class="lg:text-2xl text-xl leading-none mt-2 lg:mt-0 mb-4 font-bold" :class="{ 'text-green-500': results[1] >= 8, 'text-orange-500': results[1] >= 6 && results[1] <= 7, 'text-red-500': results[1] < 6 }">{{ results[1] }} балла(ов) из 9 — {{ summaryText(results[1]) }}</div>
        </div>

        <p class="mb-8">Первый порок – это взаимное недоверие членов команды. Как правило, оно возникает из боязни продемонстрировать перед всеми свою уязвимость, слабость. Если члены команды опасаются открыто признаться в своих ошибках и недостатках и скрывают их любой ценой, создается атмосфера взаимного недоверия и подозрительности.</p>

        <div class="leading-tight font-bold">
          <div class="lg:text-4xl text-2xl">Боязнь конфликта</div>
          <div class="lg:text-2xl text-xl leading-none mt-2 lg:mt-0 mb-4 font-bold" :class="{ 'text-green-500': results[2] >= 8, 'text-orange-500': results[2] >= 6 && results[2] <= 7, 'text-red-500': results[2] < 6 }">{{ results[2] }} балла(ов) из 9 — {{ summaryText(results[2]) }}</div>
        </div>
        <p class="mb-8">Недоверие в коллективе создает почву для развития второго порока – боязни конфликта. Люди, не доверяющие друг другу, неспособны к искренней, открытой дискуссии, боятся задеть кого-либо или оказаться задетыми. Вместо этого они ведут осторожный обмен нейтральными замечаниями, осторожными комментариями, порой переходя к ядовитым колкостям, и в результате принимают единогласное решение, которое никого не устраивает и ни к чему не ведет.</p>

        <div class="leading-tight font-bold">
          <div class="lg:text-4xl text-2xl">Безответственность</div>
          <div class="lg:text-2xl text-xl leading-none mt-2 lg:mt-0 mb-4 font-bold" :class="{ 'text-green-500': results[3] >= 8, 'text-orange-500': results[3] >= 6 && results[3] <= 7, 'text-red-500': results[3] < 6 }">{{ results[3] }} балла(ов) из 9 — {{ summaryText(results[3]) }}</div>
        </div>
        <p class="mb-8">Мнимая гармония крайне опасна, потому что порождает третий порок команды – безответственность. Поскольку в команде царит атмосфера недоверия, никто не говорит то, что думает, а решения принимаются для проформы. И члены команды считают, что не несут никакой ответственности за все то, что не затрагивает их лично.</p>

        <div class="leading-tight font-bold">
          <div class="lg:text-4xl text-2xl">Нетребовательность</div>
          <div class="lg:text-2xl text-xl leading-none mt-2 lg:mt-0 mb-4 font-bold" :class="{ 'text-green-500': results[4] >= 8, 'text-orange-500': results[4] >= 6 && results[4] <= 7, 'text-red-500': results[4] < 6 }">{{ results[4] }} балла(ов) из 9 — {{ summaryText(results[4]) }}</div>
        </div>
        <p class="mb-8">Принимая решения, к выполнению которых они равнодушны, члены команды не считают ни себя, ни своих коллег связанными какими-либо обязательствами. Нетребовательность – четвертый порок команды. Даже понимая, что какие-то действия (или, наоборот, бездеятельность) вредны для компании, сотрудники часто исходят из принципа «это не мое дело».</p>

        <div class="leading-tight font-bold">
          <div class="lg:text-4xl text-2xl">Безразличие к результатам</div>
          <div class="lg:text-2xl text-xl leading-none mt-2 lg:mt-0 mb-4 font-bold" :class="{ 'text-green-500': results[5] >= 8, 'text-orange-500': results[5] >= 6 && results[5] <= 7, 'text-red-500': results[5] < 6 }">{{ results[5] }} балла(ов) из 9 — {{ summaryText(results[5]) }}</div>
        </div>
        <p class="mb-8">Безответственность и нетребовательность создают почву для развития пятого порока. Безразличие к результатам возникает, когда члены команды ставят свои личные потребности (честолюбие, карьеру, признание) или потребности своих отделов выше общих целей команды или компании.</p>

        <div class="text-center">
          <div @click="startAgain" class="inline-block py-2 px-4 font-bold text-white rounded-lg cursor-pointer mt-4 mb-4" style="background-color: #00a8ff;">Пройти тест ещё раз</div>

          <div class="mb-6 text-sm text-gray-500">
            За изображение на главной спасибо <a href="https://www.openpeeps.com/" target="_blank" class="underline">https://www.openpeeps.com/</a>
          </div>
        </div>
      </div>
    </div>

    <a href="https://mbyakow.com" class="no-select block pt-2 pb-4 fixed bottom-0 w-full bg-white text-gray-600 hover:text-black">
      Сделал <div class="bg-cover w-6 h-6 rounded-full inline-block ml-2 mr-1 relative" style="top: .35em; background-image: url(https://avatars2.githubusercontent.com/u/7271758?s=460&u=6145cf5b34660d2405d4a2e49f09e3e03566152f&v=4);"></div> Максим Бяков
    </a>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      step: 16,
      results: {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
        5: 0,
      },
      questions: [
        {
          id: 1,
          title: 'Члены нашей команды горячо и открыто обсуждают любые вопросы и проблемы',
          type: 2,
        },
        {
          id: 2,
          title: 'Члены нашей команды открыто критикуют друг друга за недостатки и непродуктивное поведение',
          type: 4,
        },
        {
          id: 3,
          title: 'Члены нашей команды знают, над чем работают их коллеги и какой вклад они вносят в достижение общей цели команды',
          type: 3,
        },
        {
          id: 4,
          title: 'Члены нашей команды искренне и сразу же приносят извинения, если им случается задеть кого-то из коллег или непреднамеренно нанести ущерб командной работе',
          type: 1,
        },
        {
          id: 5,
          title: 'Члены нашей команды готовы пожертвовать чем-то (премией, славой, штатной единицей) ради блага всей команды',
          type: 5,
        },
        {
          id: 6,
          title: 'Члены нашей команды открыто признают свои слабости и ошибки',
          type: 1,
        },
        {
          id: 7,
          title: 'Совещания нашей команды очень интересны, на них никогда не бывает скучно',
          type: 2,
        },
        {
          id: 8,
          title: 'Члены нашей команды после совещания уверены в том, что их коллеги полностью поддерживают принятые решения и будут их выполнять, даже если сначала не были согласны с ними',
          type: 3,
        },
        {
          id: 9,
          title: 'Атмосфера в нашей команде в значительной степени зависит от успеха в достижении целей',
          type: 5,
        },
        {
          id: 10,
          title: 'На совещаниях нашей команды непременно рассматриваются самые важные и самые трудные вопросы; по ним обязательно принимаются конкретные решения',
          type: 2,
        },
        {
          id: 11,
          title: 'Члены нашей команды делают все, чтобы не подвести своих коллег',
          type: 4,
        },
        {
          id: 12,
          title: 'Члены нашей команды знают все о личной жизни друг друга и спокойно обсуждают ее',
          type: 1,
        },
        {
          id: 13,
          title: 'Члены нашей команды заканчивают обсуждение всех вопросов четкими и ясными резолюциями',
          type: 3,
        },
        {
          id: 14,
          title: 'Члены нашей команды контролируют выполнение планов и качество работы друг друга',
          type: 4,
        },
        {
          id: 15,
          title: ' Члены нашей команды не хвастаются своими достижениями, но с удовольствием признают успехи коллег',
          type: 5,
        },
      ],
    };
  },

  methods: {
    answer(value) {
      if (this.step <= 15) {
        const type = this.questions[this.step - 1].type;

        this.results[type] = this.results[type] + value;
      }

      this.step++;
    },

    summaryText(value) {
      if (value >= 8) {
        return 'этого порока у вас нет';
      }

      if (value >= 6 && value <= 7) {
        return 'есть угроза развития порока';
      }

      return 'надо срочно принимать меры';
    },

    startAgain() {
      this.step = 0;

      this.results = {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
        5: 0,
      };
    }
  },
}
</script>

<style>
#app {
  font-family: "Proxima Nova", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
}

.intro {
  font-size: 50px;
}

.subheader {
  font-size: 30px;
}

.no-select {
  user-select: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -khtml-user-select: none;
  -ms-user-select: none;
}
</style>
