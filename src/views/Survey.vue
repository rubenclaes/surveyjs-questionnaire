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
    Survey
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
                  imageWidth: 400
                },
                {
                  type: "html",
                  name: "question3",
                  html: {
                    nl:
                      "<article class='intro'>   <div class='intro__body wysiwyg'>       <p>In this section, you will be asked about your current employment and any other way you and your partner currently receive income.</p>       <p>It will be handy to have the following in front of you:</p>       <ul>        \t<li>        \t\tPayslip (for employment details)        \t</li>        \t<li>        \t\t<p>A current Centrelink Schedule for any account based pension from super, annuities, or other income stream products that you own</p>        \t\t<p>        \t\t\tIf you don't have a current one you can get these schedules by contacting your income stream provider        \t\t</p>        \t</li>        \t<li>        \t\tLatest statement from any payments (from Centrelink or other authority)        \t</li>       </ul>         </div> </article>"
                  }
                }
              ],
              readOnly: true
            }
          ]
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
                { value: "1", text: { nl: "1: Sterk oneens" } },
                { value: "2", text: { nl: "2: Oneens" } },
                { value: "3", text: { nl: "3: Neutraal" } },
                { value: "4", text: { nl: "4: Eens" } },
                { value: "5", text: { nl: "5: Helemaal eens" } }
              ],
              rows: [
                {
                  value: "row1",
                  text: {
                    nl: "1. Ik ben vaak degene die een gesprek aanknoopt."
                  }
                },
                {
                  value: "row2",
                  text: {
                    nl:
                      "2. Ik vind het absoluut geen probleem om in het middelpunt van de belangstelling te staan."
                  }
                },
                { value: "row3", text: { nl: "3. Ik praat weinig. " } },
                {
                  value: "row4",
                  text: { nl: "4. Ik barst van de energie. " }
                },
                {
                  value: "row5",
                  text: {
                    nl: "5. Ik steek anderen altijd aan met mijn enthousiasme."
                  }
                },
                {
                  value: "row6",
                  text: { nl: "6. Ik hou ervan om alleen te zijn." }
                },
                {
                  value: "row7",
                  text: {
                    nl:
                      "7. Ik heb liever een handjevol echte vrienden dan een leger aan vrienden. "
                  }
                },
                {
                  value: "row8",
                  text: {
                    nl:
                      "8. Ik geef de voorkeur aan één-op-één gesprekken boven groepsactiviteiten"
                  }
                },
                {
                  value: "row9",
                  text: {
                    nl:
                      "9. Op feestjes of sociale gelegenheden heb ik de tijd van mijn leven. "
                  }
                },
                {
                  value: "row10",
                  text: { nl: "10. Ik denk niet na voor ik iets zeg." }
                },
                {
                  value: "row11",
                  text: {
                    nl: "11. Mensen zeggen dat ik een goede luisteraar ben. "
                  }
                },
                {
                  value: "row12",
                  text: {
                    nl: "12. Ik vind het fijn om nieuwe mensen te leren kennen."
                  }
                },
                {
                  value: "row13",
                  text: { nl: "13. Ik neem niet graag grote risico’s." }
                },
                {
                  value: "row14",
                  text: {
                    nl:
                      "14. Ik neem niet graag de telefoon op, maar laat mensen liever mijn voicemail inspreken. "
                  }
                }
              ],
              isAllRowRequired: true
            }
          ],
          questionTitleLocation: "top",
          title: {
            nl: "Geef aan in hoeverre je het eens bent met elke stelling."
          },
          description: {
            nl:
              "1 = Sterk oneens\n2 = Oneens\n3 = Neutraal\n4 = Eens\n5 = Helemaal eens\n"
          }
        }
      ],
      calculatedValues: [{ name: "Extrovert" }],
      sendResultOnPageNext: true,
      showQuestionNumbers: "off",
      pagePrevText: { nl: "Vorige" },
      pageNextText: { nl: "Volgende" },
      questionTitlePattern: "numTitle",
      firstPageIsStarted: true
    };

    var model = new SurveyVue.Model(json);

    model.onComplete.add(function(result) {
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
        row14
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
          intitial = "Klein verschil, geen duidelijke voorkeur";
          break;
        case absoluteScore >= 5 && absoluteScore <= 15:
          console.log("Gemiddeld verschil");
          intitial = "Gemiddeld verschil, beperkte voorkeur";
          break;
        case absoluteScore > 15:
          console.log("Groot verschil");
          intitial = "Groot verschil, duidelijke voorkeur";
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
          text: intitial
        }
      });
    });

    return {
      survey: model
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
