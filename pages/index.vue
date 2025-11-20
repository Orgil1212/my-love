<template>
  <div class="container">
    <div v-if="!revealed" id="first-stage" class="card-glow">
      <h1>💌 Нэгэн Нууц Хайрын Захидал</h1>
      <p class="subtitle">Энэхүү захидлыг зөвхөн Танд зориулан маш нууцаар илгээв.</p>
      <button @click="revealMessage" id="reveal-btn">
        <span class="heart-pulse">💖</span> Задлаад Унш
      </button>
    </div>

    <div v-else id="second-stage" class="card-glow fade-in">
      <h2>✨ Миний Сэтгэлийн Гүнээс...</h2>
      <p class="love-message special-font">
        Би чамтай учирсан цагаасаа хойш миний амьдрал гэрэл гэгээтэй, утга учиртай болсон.
        Чи миний хамгийн хөгжилтэй, хамгийн ухаалаг, хамгийн сайхан охин. 
        <br>
        Миний зүрх чамгүйгээр цохилох аргагүй болжээ. Бидний түүхийг хамтдаа эхлүүлэхийг хүсэж байна.
        <br><br>
        **Хайрт минь, надтай үерхээч?**
      </p>

      <div v-if="!answered" class="choice-buttons">
        <button @click="submitChoice('Тэгий')" class="choice-btn yes-btn">💍 Тийм! Би зөвшөөрч байна.</button>
        <button @click="submitChoice('Үгүй')" class="choice-btn no-btn">💔 Үгүй... Уучлаарай.</button>
      </div>

      <p v-else id="thank-you-message" class="special-font">
        {{ answerMessage }}
        <span v-if="isSubmitting" style="margin-left: 10px;">...Илгээлт амжилттай</span>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// 1. Төлөвийн Хувьсагчууд
const revealed = ref(false); // Захидал задалсан эсэх
const answered = ref(false); // Хариулсан эсэх
const isSubmitting = ref(false); // Илгээлт хийгдэж байгаа эсэх
const answerMessage = ref(''); // Хариултын дараах мессеж

// 2. Үйлдэл (Methods)
const revealMessage = () => {
  revealed.value = true;
};

const submitChoice = async (choice) => {
  if (answered.value || isSubmitting.value) return;

  isSubmitting.value = true;
  answered.value = true;

  // Хариултын мессежийг өөрчилсөн
  if (choice === 'Тийм') {
    answerMessage.value = 'Амьдралын минь хамгийн сайхан хариулт! Намайг хамгийн аз жаргалтай хүн болгосонд баярлалаа! Үүрд хамтдаа. ❤️';
  } else {
    answerMessage.value = 'Баярлалаа. Би таны сонголтыг хүндэтгэнэ. Хэдийгээр миний зүрх өвдөж байгаа ч, би таны аз жаргалыг хүсэж байна. Та сайхан яваарай. 🥺';
  }

  // Мэйл илгээх хэсэг - ЭНЭ КОД ЗӨВ БАЙНА
  try {
    const formspreeEndpoint = 'https://formspree.io/f/xkgyeoby'; 

    const response = await fetch(formspreeEndpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify({
        answer: choice,
        page: 'Dating Proposal'
      })
    });

    if (!response.ok) {
        console.error("Mail send error:", response.statusText);
    }

  } catch (error) {
    console.error("Network error during mail submission:", error);
  } finally {
    isSubmitting.value = false;
  }
};
</script>

<style scoped>
/* 3. Шинэчилсэн CSS */
/* Фонт: Google Fonts-оос гоё фонт нэмэх (Жишээлбэл, Merienda) - Үүнийг Nuxt-ийн config эсвэл main CSS-д нэмэх шаардлагатай */

.container {
    font-family: 'Georgia', serif; /* Бага зэрэг романтик фонт */
    background: linear-gradient(135deg, #ffdde1 0%, #ee9ca7 100%); /* Градиент дэвсгэр */
    color: #333;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
    text-align: center;
}

#first-stage, #second-stage {
    background-color: rgba(255, 255, 255, 0.95); /* Бага зэргийн тунгалаг цагаан */
    padding: 40px;
    border-radius: 20px; /* Илүү бөөрөнхий ирмэг */
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    max-width: 550px;
    width: 90%;
    border: 3px solid #ff99cc; /* Нэмэлт хүрээ */
}

h1 { color: #d63384; font-size: 2.2em; } /* Гүн ягаан */
h2 { color: #a02046; font-size: 1.8em; margin-bottom: 25px; } /* Улаан туяатай */
.subtitle { color: #888; margin-bottom: 30px; font-style: italic; }

.special-font {
    font-family: 'Times New Roman', serif;
    font-style: italic;
    font-weight: 500;
}

.love-message {
    font-size: 1.3em;
    line-height: 1.7;
    margin-bottom: 40px;
    border-left: 5px solid #ff66b2;
    padding-left: 20px;
    text-align: left;
    background-color: #fff0f5; /* Нэмэлт дэвсгэр */
    padding: 15px;
    border-radius: 10px;
}

/* Товчлуурын загвар */
#reveal-btn {
    background-color: #ff66b2;
    color: white;
    padding: 15px 35px;
    border: none;
    border-radius: 50px;
    font-size: 1.2em;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
    box-shadow: 0 4px 10px rgba(255, 102, 178, 0.4);
}

#reveal-btn:hover {
    background-color: #e60073;
    transform: translateY(-2px);
}

.choice-btn {
    padding: 15px 30px;
    margin: 0 15px;
    border: none;
    border-radius: 30px; /* Илүү бөөрөнхий */
    font-weight: bold;
    cursor: pointer;
    font-size: 1.1em;
    transition: transform 0.3s, box-shadow 0.3s;
}

.yes-btn {
    background-color: #4CAF50; /* Ногоон */
    color: white;
}

.no-btn {
    background-color: #f44336; /* Улаан */
    color: white;
}

.choice-btn:hover {
    transform: scale(1.05); /* Томрох */
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.2);
}

#thank-you-message {
    font-size: 1.4em;
    font-weight: bold;
    color: #a02046;
    padding: 20px;
    border: 3px solid #ff99cc;
    border-radius: 15px;
    background-color: #fff5fa;
}

/* Анимац */
@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

.heart-pulse {
    display: inline-block;
    animation: pulse 1s infinite; /* Зүрхний цохилт */
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
}

.fade-in {
    animation: fadeIn 0.8s ease-out; /* Зөөлөн гарч ирэх */
}
</style>