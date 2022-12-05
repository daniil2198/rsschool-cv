# Daniil Peskouski

## Contacts
Telegram: @peskouski

## About me
My target - will be Frontend Developer. I work like Data Entry in one of the Shopify theme development team.

For example, my first JS code for calories calculator.

"use strict";
const counter = document.querySelector(".counter");
// Данные от пользователя
const weight = document.getElementById("weight");
const height = document.getElementById("height");
const age = document.getElementById("age");
const maleGender = document.getElementById("gender-male");
const femaleGender = document.getElementById("gender-female");
const activityConstants = {
  min: 1.2,
  low: 1.375,
  medium: 1.55,
  high: 1.725,
  max: 1.9,
};
const activityCheckboxes = document.querySelectorAll(".radio > div > input");
const activityCheckboxesBox = document.querySelector(".radios-group");

// Кнопки "Рассчитать" и "Сбросить"
const calculateButton = document.querySelector(".form__submit-button");
const resetButton = document.querySelector(".form__reset-button");

// Окно с итогами
let resultBlockNode;

function getActivityConstant() {
  let activityConstant;
  activityCheckboxes.forEach((checkbox) => {
    if (checkbox.hasAttribute("checked")) {
      activityConstant = activityConstants[checkbox.value];
    }
  });
  return activityConstant;
}
function calculateCalories() {
  let qualityCalories;
  if (maleGender.hasAttribute("checked")) {
    qualityCalories =
      10 * weight.value + 6.25 * height.value - 5 * age.value + 5;
  }
  if (!maleGender.hasAttribute("checked")) {
    qualityCalories =
      10 * weight.value + 6.25 * height.value - 5 * age.value - 161;
  }
  return qualityCalories * getActivityConstant();
}
function checkInput() {
  if (isFinite(Number(this.value))) {
  } else {
    this.value = age.value.slice(0, -1);
  }
  if (weight.value && age.value && height.value) {
    calculateButton.removeAttribute("disabled");
  }
  if (weight.value || age.value || height.value) {
    resetButton.removeAttribute("disabled");
  }
}
function checkRangeAge() {
  if (age.value < 10 || age.value > 75) {
    alert("Разрешенный возраст от 10 до 75 лет");
    age.focus();
    return false;
  }
  return true;
}
function checkRangeWeight() {
  if (weight.value < 25 || weight.value > 200) {
    alert("Разрешенный вес от 25 до 200 кг");
    weight.focus();
    return false;
  }
  return true;
}
function checkRangeHeight() {
  if (height.value < 50 || height.value > 220) {
    alert("Разрешенный рост от 50 до 220 см");
    height.focus();
    return false;
  }
  return true;
}
function changeGender() {
  maleGender.removeAttribute("checked");
  femaleGender.removeAttribute("checked");
  this.setAttribute("checked", "");
}
function changeActivity(e) {
  activityCheckboxes.forEach((checkbox) => {
    checkbox.removeAttribute("checked");
  });
  e.target.setAttribute("checked", "");
}
function renderResultBlock() {
  const caloriesNorm = Math.trunc(calculateCalories());
  const caloriesMin = Math.trunc(calculateCalories() * 0.85);
  const caloriesMax = Math.trunc(calculateCalories() * 1.15);
  const resultBlock = `
  <section class="counter__result">
          <h2 class="heading">
            Ваша норма калорий
          </h2>
          <ul class="counter__result-list">
            <li class="counter__result-item">
              <h3>
                <span id="calories-norm">${caloriesNorm}</span> ккал
              </h3>
              <p>
                поддержание веса
              </p>
            </li>
            <li class="counter__result-item">
              <h3>
                <span id="calories-minimal">${caloriesMin}</span> ккал
              </h3>
              <p>
                снижение веса
              </p>
            </li>
            <li class="counter__result-item">
              <h3>
                <span id="calories-maximal">${caloriesMax}</span> ккал
              </h3>
              <p>
                набор веса
              </p>
            </li>
          </ul>
        </section>
  `;
  counter.insertAdjacentHTML("beforeend", resultBlock);
}

// Событие проверки ввода значения с клавиатуры в инпуты характеристик
age.addEventListener("keyup", checkInput);
weight.addEventListener("keyup", checkInput);
height.addEventListener("keyup", checkInput);

const checkRangeAgeEvent = age.addEventListener("change", checkRangeAge);
const checkRangeWeightEvent = weight.addEventListener(
  "change",
  checkRangeWeight
);
const checkRangeHeightEvent = height.addEventListener(
  "change",
  checkRangeHeight
);

// Смена значений чекбоксов пола и физической активности
maleGender.addEventListener("click", changeGender);
femaleGender.addEventListener("click", changeGender);

activityCheckboxesBox.addEventListener("click", changeActivity);

// Генерация вывода блока с результатами
calculateButton.addEventListener("click", function (e) {
  e.preventDefault();
  resultBlockNode = document.querySelector(".counter__result");
  if (!resultBlockNode) {
    if (!checkRangeAge() && !checkRangeHeight() && !checkRangeWeight()) return;
    console.log("goo");
    renderResultBlock();
  } else {
    resultBlockNode.remove();
    if (!checkRangeAge() && !checkRangeHeight() && !checkRangeWeight()) return;
    renderResultBlock();
  }
});

// Кнопка очистки полей, приведение в дефолтное состояние
resetButton.addEventListener("click", function (e) {
  // Установка кнопок расчета и сброса в дизейбл
  calculateButton.setAttribute("disabled", "");
  resetButton.setAttribute("disabled", "");

  // Установка нулевых значений в инпуты
  weight.value = "";
  height.value = "";
  age.value = "";

  // Установка минимальной активности по умолчанию
  const [defaultActivityCheckbox] = activityCheckboxes;
  activityCheckboxes.forEach((checkbox) => {
    checkbox.removeAttribute("checked");
  });
  defaultActivityCheckbox.setAttribute("checked", "");

  // Установка мужского пола по умолчанию
  femaleGender.removeAttribute("checked");
  maleGender.setAttribute("checked", "");

  // Проверка на существование окна с результатами
  resultBlockNode = document.querySelector(".counter__result");
  if (resultBlockNode) {
    resultBlockNode.remove();
  }
});
