<template>
  <div class="container">
    <survey :survey="survey"></survey>
    <div id="surveyResult"></div>
  </div>
</template>

<script>
import * as SurveyVue from "survey-vue";
import "bootstrap/dist/css/bootstrap.css";

import { router } from "../App";

var Survey = SurveyVue.Survey;
Survey.cssType = "bootstrap";

import * as widgets from "surveyjs-widgets";

import { init as customWidget } from "../components/customwidget";

widgets.icheck(SurveyVue);

customWidget(SurveyVue);

export default {
  components: {
    Survey,
  },

  computed: {
    // a computed getter
  },
  data() {
    var json = {
      locale: "nl",
      title: { nl: "Test: Introvert versus Extravert" },
      logoWidth: 200,
      pages: [
        {
          name: "voorwoord",
          elements: [
            {
              type: "panel",
              name: "panel1",
              elements: [
                {
                  type: "image",
                  name: "image",
                  imageLink:
                    "https://miro.medium.com/max/701/1*upFupNHCNiTO6ltvJCukxg.png",

                  imageHeight: 400,
                  imageWidth: 400,
                },
                {
                  type: "html",
                  name: "question3",
                  html: {
                    nl:
                      "<article class='intro'>   <div class='intro__body wysiwyg'>       <p>Doe de test: ben je introvert of extravert? Klik op start en kom het te weten!</p>",
                  },
                },
              ],
              readOnly: true,
            },
          ],
        },
        {
          name: "Questionnaire",
          elements: [
            {
              type: "matrix",
              name: "questionnaire",
              title: { nl: "Vragen" },
              hideNumber: true,
              isRequired: true,
              requiredErrorText: { nl: "Vul alle vragen in." },
              columns: [
                { value: "1", text: { nl: "Sterk oneens" } },
                { value: "2", text: { nl: "Oneens" } },
                { value: "3", text: { nl: "Neutraal" } },
                { value: "4", text: { nl: "Eens" } },
                { value: "5", text: { nl: "Helemaal eens" } },
              ],
              rows: [
                {
                  value: "row1",
                  text: {
                    nl: "1. Ik ben vaak degene die een gesprek aanknoopt.",
                  },
                },
                {
                  value: "row2",
                  text: {
                    nl:
                      "2. Ik vind het absoluut geen probleem om in het middelpunt van de belangstelling te staan.",
                  },
                },
                { value: "row3", text: { nl: "3. Ik praat weinig. " } },
                {
                  value: "row4",
                  text: { nl: "4. Ik barst van de energie. " },
                },
                {
                  value: "row5",
                  text: {
                    nl: "5. Ik steek anderen altijd aan met mijn enthousiasme.",
                  },
                },
                {
                  value: "row6",
                  text: { nl: "6. Ik hou ervan om alleen te zijn." },
                },
                {
                  value: "row7",
                  text: {
                    nl:
                      "7. Ik heb liever een handjevol echte vrienden dan een leger aan vrienden. ",
                  },
                },
                {
                  value: "row8",
                  text: {
                    nl:
                      "8. Ik geef de voorkeur aan één-op-één gesprekken boven groepsactiviteiten",
                  },
                },
                {
                  value: "row9",
                  text: {
                    nl:
                      "9. Op feestjes of sociale gelegenheden heb ik de tijd van mijn leven. ",
                  },
                },
                {
                  value: "row10",
                  text: { nl: "10. Ik denk niet na voor ik iets zeg." },
                },
                {
                  value: "row11",
                  text: {
                    nl: "11. Mensen zeggen dat ik een goede luisteraar ben. ",
                  },
                },
                {
                  value: "row12",
                  text: {
                    nl:
                      "12. Ik vind het fijn om nieuwe mensen te leren kennen.",
                  },
                },
                {
                  value: "row13",
                  text: { nl: "13. Ik neem niet graag grote risico’s." },
                },
                {
                  value: "row14",
                  text: {
                    nl:
                      "14. Ik neem niet graag de telefoon op, maar laat mensen liever mijn voicemail inspreken. ",
                  },
                },
              ],
              isAllRowRequired: true,
            },
          ],
          questionTitleLocation: "top",
          title: {
            nl: "Geef aan in hoeverre je het eens bent met elke stelling.",
          },
        },
      ],
      calculatedValues: [{ name: "Extrovert" }],
      sendResultOnPageNext: true,
      showQuestionNumbers: "off",
      pagePrevText: { nl: "Vorige" },
      pageNextText: { nl: "Volgende" },
      questionTitlePattern: "numTitle",
      firstPageIsStarted: true,
    };

    var model = new SurveyVue.Model(json);

    model.onComplete.add(function (result) {
      var surveyData = result.data;
      const {
        row1,
        row2,
        row3,
        row4,
        row5,
        row6,
        row7,
        row8,
        row9,
        row10,
        row11,
        row12,
        row13,
        row14,
      } = surveyData.questionnaire;

      let extravertScore =
        parseInt(row1) +
        parseInt(row2) +
        parseInt(row4) +
        parseInt(row5) +
        parseInt(row9) +
        parseInt(row10) +
        parseInt(row12);
      let introvertScore =
        parseInt(row3) +
        parseInt(row6) +
        parseInt(row7) +
        parseInt(row8) +
        parseInt(row11) +
        parseInt(row13) +
        parseInt(row14);

      let absoluteScore = Math.abs(introvertScore - extravertScore);

      let intitial;
      switch (true) {
        case absoluteScore < 5:
          console.log("Klein verschil");
          intitial = `Je scoort ongeveer even hoog op zowel 'introvert' als 'extravert'. Je hebt geen duidelijke voorkeur, je bent niet opvallend introvert of extravert. Dit noemt men ook wel ambivert: je kunt jezelf niet echt introvert, noch extravert noemen. Dit varieert naargelang het moment en context. Net als een kameleon kun je je makkelijk aan andere mensen aanpassen, want moeilijk zit je niet in elkaar. Soms ga je bewust op zoek naar sociaal contact, terwijl je op andere momenten liever even alleen gelaten wordt. Stabiliteit zit in jouw DNA, en als er iemand een luisterend oor nodig heeft, zit die bij jou aan het juiste adres.`;
          break;
        case absoluteScore >= 5 &&
          absoluteScore <= 15 &&
          extravertScore > introvertScore:
          console.log("Gemiddeld verschil");
          intitial =
            "Je bent eerder extravert dan introvert. Hoewel je geen enorm uitgesproken voorkeur hebt, ben je toch liever in het gezelschap van anderen dan dat je alleen bent. Je houdt het meest van sociale bezigheden en praat graag en veel. Af en toe heb je toch ook nood aan wat rust.";
          break;
        case absoluteScore >= 5 &&
          absoluteScore <= 15 &&
          introvertScore > extravertScore:
          console.log("Gemiddeld verschil");
          intitial =
            "Je bent eerder introvert dan extravert: je vindt het fijn om de rust te vinden door bv. op je kamer muziek te luisteren of een boek te lezen. Je hebt liever een rustige avond met een paar goede vrienden dan naar een druk en groot feest te gaan. Sociaal contact vind je niet erg, maar continu nieuwe mensen leren kennen hoeft niet voor jou.";
          break;
        case absoluteScore > 15 && extravertScore > introvertScore:
          console.log("Groot verschil");
          intitial =
            "Je bent uitgesproken extravert. Mensen omschrijven jou als een enthousiaste flapuit die zijn medemens maar zelden met rust laat. Je houdt van veel sociaal contact en babbelt er graag op los. Zonder mensen om je heen voel je je wat verloren.";
          break;
        case absoluteScore > 15 && introvertScore > extravertScore:
          console.log("Groot verschil");
          intitial =
            "Je bent uitgesproken introvert. Je verdwaalt graag in je eigen hoofd en je hebt weinig behoefte aan schreeuwerige mensen om zich heen. e bent rustig en gevoelig. Je luistert meer dan dat je praat en denkt goed na voordat je handelt. Je bent graag op jezelf, en net omdat sociaal contact wat moeilijker is, liggen de vrienden die je wel hebt jou des te nauwer aan het hart.";
          break;
        default:
          intitial = "Score kan niet worden bepaald.";
          break;
      }

      document.querySelector(
        "#surveyResult"
      ).textContent = `extravertScore = ${extravertScore}, intervertScore = ${introvertScore} absuluteScore = ${absoluteScore}, text = ${intitial}`;

      router.push({
        name: "completed",
        params: {
          extravertScore: extravertScore,
          introvertScore: introvertScore,
          absoluteScore: absoluteScore,
          text: intitial,
        },
      });
    });

    return {
      survey: model,
    };
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

p {
  font-size: 16px;
}
</style>
