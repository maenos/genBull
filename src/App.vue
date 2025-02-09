<template>
  <div class="min-h-screen bg-gray-100 p-4 print:p-0 print:bg-white">
    <div
      class="max-w-[21cm] mx-auto bg-white shadow-lg p-2 min-h-[29.7cm] print:shadow-none print:p-1"
    >
      <!-- Header -->
      <div class="mb-6">
        <div class="flex justify-between">
          <div>
            <p class="my-1 mb-2 text-[13px] text-center">
              Ministère de l’Enseignement Primaire <br />
              et secondaire et de la formation <br />
              professionnelle
            </p>
          </div>

          <div>
            <p class="text-sm text-center">REPUBLIQUE TOGOLAISE</p>
            <p class="text-sm text-center">TRAVAIL - LIBERTE - PATRIE</p>
          </div>
        </div>

        <div class="text-center">
          <h1 class="text-sm font-bold uppercase mb-0">{{ schoolInfo.name }}</h1>
          <p class="text-sm">{{ schoolInfo.address }}</p>
          <p class="text-sm">Tel: {{ schoolInfo.phone }}</p>
          <p class="text-sm font-bold uppercase mt-2">
            BULLETIN du {{ schoolInfo.cycle }}
          </p>
        </div>
      </div>

      <!-- Student Info -->
      <div class="grid grid-cols-2 gap-4 mb-6 text-sm">
        <div>
          <p><span class="font-bold">Matricule:</span> {{ student.id }}</p>
          <p><span class="font-bold">Nom & Prénom:</span> {{ student.name }}</p>
        </div>
        <div>
          <p><span class="font-bold">Classe:</span> {{ student.class }}</p>
          <p>
            <span class="font-bold">Année Académique:</span> {{ student.academicYear }}
          </p>
        </div>
      </div>

      <!-- Grades Table -->
      <div class="overflow-x-auto mb-6">
        <table class="w-full text-sm border-collapse text-[11px]">
          <thead>
            <tr class="bg-gray-100">
              <th class="border p-1 text-left">MATIERES</th>
              <th class="border p-1">NOTE 1</th>
              <th class="border p-1">NOTE 2</th>
              <th class="border p-1">MOY CLASSE</th>
              <th class="border p-1">COMPO</th>
              <th class="border p-1">NOTE MOY</th>
              <th class="border p-1">COEF</th>
              <th class="border p-1">NOTE COEF</th>

              <th class="border p-1">PROFESSEUR</th>
              <th class="border p-1">REMARQUES</th>
            </tr>
          </thead>
          <tbody>
            <!-- Literary Subjects -->
            <tr class="bg-gray-50">
              <td colspan="11" class="border p-1 font-bold">Matières Fondamentales</td>
            </tr>
            <tr
              v-for="subject in literarySubjects"
              :key="subject.name"
              class="text-[11px]"
            >
              <td class="border p-1">{{ subject.name }}</td>
              <td class="border p-1 text-end">{{ subject.note1 }}</td>
              <td class="border p-1 text-end">{{ subject.note2 }}</td>
              <td class="border p-1 text-end">
                {{ ((subject.note1 + subject.note2) / 2).toFixed(2) }}
              </td>
              <td class="border p-1 text-end">{{ subject.exam }}</td>
              <td class="border p-1 text-end">
                {{ ((subject.note1 + subject.note2 + subject.exam) / 3).toFixed(2) }}
              </td>
              <td class="border p-1 text-end">{{ subject.coefficient }}</td>
              <td class="border p-1 text-end">
                {{
                  (
                    ((subject.note1 + subject.note2 + subject.exam) *
                      subject.coefficient) /
                    3
                  ).toFixed(2)
                }}
              </td>

              <td class="border p-1">{{ subject.teacher }}</td>
              <td class="border p-1 text-[8px]">
                {{
                  getRemarks(
                    ((subject.note1 + subject.note2 + subject.exam) / 3).toFixed(2)
                  )
                }}
              </td>
            </tr>

            <!-- Scientific Subjects -->
            <tr class="bg-gray-50">
              <td colspan="11" class="border p-1 font-bold" v-if="false">
                Matières scientifiques
              </td>
            </tr>
            <tr
              v-if="false"
              v-for="subject in scientificSubjects"
              :key="subject.name"
              class="text-[11px]"
            >
              <td class="border p-1">{{ subject.name }}</td>
              <td class="border p-1 text-end">{{ subject.note1 }}</td>
              <td class="border p-1 text-end">{{ subject.note2 }}</td>
              <td class="border p-1 text-end">
                {{ ((subject.note1 + subject.note2) / 2).toFixed(2) }}
              </td>
              <td class="border p-1 text-end">{{ subject.exam }}</td>
              <td class="border p-1 text-end">
                {{ ((subject.note1 + subject.note2 + subject.exam) / 3).toFixed(2) }}
              </td>
              <td class="border p-1 text-end">{{ subject.coefficient }}</td>
              <td class="border p-1 text-end">
                {{
                  (
                    ((subject.note1 + subject.note2 + subject.exam) *
                      subject.coefficient) /
                    3
                  ).toFixed(2)
                }}
              </td>

              <td class="border p-1">{{ subject.teacher }}</td>
              <td class="border p-1 text-[8px]">
                {{
                  getRemarks(
                    ((subject.note1 + subject.note2 + subject.exam) / 3).toFixed(2)
                  )
                }}
              </td>
            </tr>

            <!-- Optional Subjects -->
            <tr class="bg-gray-50">
              <td colspan="11" class="border p-1 font-bold">Matières facultatives</td>
            </tr>
            <tr
              v-for="subject in optionalSubjects"
              :key="subject.name"
              class="text-[11px]"
            >
              <td class="border p-1">{{ subject.name }}</td>
              <td class="border p-1 text-end">{{ subject.note1 }}</td>
              <td class="border p-1 text-end">{{ subject.note2 }}</td>
              <td class="border p-1 text-end">
                {{ ((subject.note1 + subject.note2) / 2).toFixed(2) }}
              </td>
              <td class="border p-1 text-end">{{ subject.exam }}</td>
              <td class="border p-1 text-end">
                {{ ((subject.note1 + subject.note2 + subject.exam) / 3).toFixed(2) }}
              </td>
              <td class="border p-1 text-end">{{ subject.coefficient }}</td>
              <td class="border p-1 text-end">
                {{
                  (
                    ((subject.note1 + subject.note2 + subject.exam) *
                      subject.coefficient) /
                    3
                  ).toFixed(2)
                }}
              </td>

              <td class="border p-1">{{ subject.teacher }}</td>
              <td class="border p-1 text-[8px]">
                {{
                  getRemarks(
                    ((subject.note1 + subject.note2 + subject.exam) / 3).toFixed(2)
                  )
                }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- Total General -->
      <div class="mb-6 text-sm">
        <div class="grid grid-cols-2 gap-4">
          <div>
            <p>
              <span class="font-bold">Total des coefficients:</span>
              {{ totals.coefficients.total }}
            </p>
          </div>
          <div>
            <p>
              <span class="font-bold">Total des notes coefficientées:</span>
              {{ totals.weightedGrades.total }}
            </p>
          </div>
        </div>
      </div>

      <!-- Summary -->
      <div class="mb-6 text-sm">
        <div class="grid grid-cols-3 gap-4">
          <div>
            <p><span class="font-bold">Moyenne:</span> {{ totals.generalAverage }}</p>
            <p><span class="font-bold">Rang:</span> {{ rank }}</p>
          </div>
          <div v-if="classSize">
            <p><span class="font-bold">moyen Extreme:</span> 14.01</p>
            <p><span class="font-bold">moyen Faible:</span> 8.79</p>
          </div>
          <div>
            <p><span class="font-bold">Effectif:</span> {{ classSize }}</p>
            <p>
              <span class="font-bold">Mention:</span>
              {{
                /*
                getRemarks(totals.generalAverage).toFixed(2)
                  )*/

                getRemarks(totals.generalAverage)
              }}
              {{}}
            </p>
            <p>
              <!--    <span class="font-bold">Décision:</span> {{ decision }} avec
              {{ Allaverage }} de moyenne-->
            </p>
          </div>
        </div>
      </div>

      <!-- Trimester Averages -->
      <div class="mb-6 text-sm">
        <h3 class="font-bold mb-2">Moyennes Semestrielles</h3>
        <div class="grid grid-cols-2 gap-4">
          <div>
            <p>
              <span class="font-bold">1er semestre:</span> {{ firstTrimesterAverage }}
            </p>
          </div>
          <div v-if="secondTrimesterAverage">
            <p>
              <span class="font-bold">2ème semestre:</span> {{ secondTrimesterAverage }}
            </p>
          </div>
          <div v-if="false">
            <p>
              <span class="font-bold">3ème Trimestre:</span> {{ thirdTrimesterAverage }}
            </p>
          </div>
        </div>
      </div>

      <!-- Footer -->
      <div class="text-sm mt-8">
        <div class="grid grid-cols-3 gap-4">
          <div>
            <h3 class="font-bold mb-2">Légende</h3>
            <div class="grid grid-cols-2 gap-2">
              <div>5 - Excellent</div>
              <div>4 - Très bien</div>
              <div>3 - Bien</div>
              <div>2 - Insuffisant</div>
              <div>1 - Médiocre</div>
            </div>
          </div>
          <div>
            <h3 class="font-bold mb-2">Titulaire de classe</h3>
            <p>{{ classTeacher.name }}</p>
            <div class="mt-4">Signature:</div>
            <div class="mt-2 h-10 border-b border-black"></div>
          </div>
          <div class="text-right">
            <p>Date: {{ "10/03/2023" }}</p>
            <p class="mt-4">Le Directeur</p>
            <div class="mt-2 h-10 border-b border-black"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// School Information
const schoolInfo = ref({
  name: "COMPLEXE SCOLAIRE LIDAO",
  address: "Agoe-nyivé,Sogbossito",
  phone: "00228 90421598/92083054",
  cycle: "1er semestre",
});

// Student Information
const student = ref({
  id: "LIDAO/LM/2021/1268",
  name: "EGBAREH La joie Biguedinam Charité",
  class: "1ere G1",
  academicYear: "2022-2023",
});

const literarySubjects = ref([
  {
    name: "Français",
    note1: 13,
    note2: 14.5,
    exam: 15,
    coefficient: 3,
    teacher: "LAWSON ",
    comments: "Bien",
  },
  {
    name: "ECM/HG",
    note1: 12,
    note2: 13,
    exam: 14,
    coefficient: 2,
    teacher: "BASSASSAGA ",
    comments: "Assez bien",
  },
  {
    name: "Anglais",
    note1: 12,
    note2: 12,
    exam: 13,
    coefficient: 2,
    teacher: "ABALO ",
    comments: "Assez bien",
  },
  {
    name: "Allemand",
    note1: 12,
    note2: 12,
    exam: 12.5,
    coefficient: 2,
    teacher: " SIMLIWA ",
    comments: "Assez bien",
  },
  {
    name: "Droit civil",
    note1: 15,
    note2: 14,
    exam: 15,
    coefficient: 2,
    teacher: "KABISSI ",
    comments: "Assez bien",
  },
  {
    name: "Eco. Orga",
    note1: 5,
    note2: 8,
    exam: 6,
    coefficient: 2,
    teacher: "KOUDJOM ",
    comments: "insuffisant",
  },
  {
    name: "Eco. Géné",
    note1: 8,
    note2: 8,
    exam: 7,
    coefficient: 2,
    teacher: " AMOUZOU ",
    comments: "insuffisant",
  },
  {
    name: "TAS",
    note1: 14,
    note2: 14,
    exam: 14,
    coefficient: 4,
    teacher: "KONDO ",
    comments: "Assez bien",
  },
  {
    name: "TBS",
    note1: 16,
    note2: 18,
    exam: 18,
    coefficient: 1,
    teacher: "TOKO ",
    comments: "Assez bien",
  },
  {
    name: "Bureautique",
    note1: 18,
    note2: 19,
    exam: 19,
    coefficient: 1,
    teacher: "DOUTI ",
    comments: "T. bien",
  },
  {
    name: "EPS",
    note1: 12,
    note2: 12,
    exam: 15,
    coefficient: 1,
    teacher: " MANAKE",
    comments: "Assez bien",
  },
]);

const scientificSubjects = ref([
  {
    name: "Mathématiques",
    note1: 10,
    note2: 11,
    exam: 13,
    coefficient: 4,
    teacher: " TONOU",
    comments: "Passable",
  },
  {
    name: "SVT",
    note1: 10,
    note2: 12,
    exam: 11,
    coefficient: 1,
    teacher: " KOLANI",
    comments: "Passable",
  },
  {
    name: "SP",
    note1: 10,
    note2: 11,
    exam: 11,
    coefficient: 1,
    teacher: " IDRISSOU",
    comments: "Passable",
  },
]);

const optionalSubjects = ref([
  {
    name: "EM",
    note1: 10,
    note2: 10,
    exam: 10,
    coefficient: 1,
    teacher: "KPADJIBA ",
    comments: "Passable",
  },
]);
/*
const literarySubjects = ref([
  {
    name: "Français",
    note1: 9,
    note2: 10,
    exam: 10,
    coefficient: 4,
    teacher: "M. BATAKO",
    comments: "Passable",
  },
  {
    name: "ECM",
    note1: 10,
    note2: 9,
    exam: 9,
    coefficient: 2,
    teacher: "M. BATAKO",
    comments: "Peut mieux faire",
  },
  {
    name: "Anglais",
    note1: 8,
    note2: 9,
    exam: 10,
    coefficient: 3,
    teacher: "M. FOUSSENI",
    comments: "Des efforts à fournir",
  },
  {
    name: "Allemand",
    note1: 8,
    note2: 9,
    exam: 9,
    coefficient: 2,
    teacher: "M. OURO-SEVA",
    comments: "Doit redoubler d'efforts",
  },
  {
    name: "Histo-geo",
    note1: 10,
    note2: 9,
    exam: 8,
    coefficient: 3,
    teacher: "M. BASSAGA",
    comments: "Insuffisant",
  },
]);

const scientificSubjects = ref([
  {
    name: "Mathématiques",
    note1: 8,
    note2: 9,
    exam: 10,
    coefficient: 4,
    teacher: "Mr TONOU",
    comments: "Doit réviser davantage",
  },
  {
    name: "SVT",
    note1: 9,
    note2: 10,
    exam: 9,
    coefficient: 4,
    teacher: "Mr KOLANI",
    comments: "Efforts insuffisants",
  },
  {
    name: "SP",
    note1: 8,
    note2: 9,
    exam: 10,
    coefficient: 4,
    teacher: "Mr IDRISSOU",
    comments: "Doit s'améliorer",
  },
]);

const optionalSubjects = ref([
  {
    name: "EPS",
    note1: 12,
    note2: 13,
    exam: 13,
    coefficient: 1,
    teacher: "M. MANAKE",
    comments: "Assez bien",
  },
  {
    name: "EM",
    note1: 10,
    note2: 9,
    exam: 10,
    coefficient: 1,
    teacher: "M. KPADJIBA",
    comments: "Passable",
  },
]);
*/
// Computed Properties
const allSubjects = computed(() => [
  ...optionalSubjects.value,
  ...literarySubjects.value,
  //...scientificSubjects.value
]);

const totalCoefficients = computed(() => {
  return allSubjects.value.reduce((sum, subject) => sum + subject.coefficient, 0);
});

console.log(totalCoefficients.value);

const totalWeightedGrades = computed(() => {
  return allSubjects.value.reduce((sum, subject) => sum + subject.weightedGrade, 0);
});

const generalAverage = computed(() => {
  return (totalWeightedGrades.value / totalCoefficients.value).toFixed(2);
});

const rank = computed(() => {
  return "2e";
});

const classSize = ref(9);
const decision = ref("Admis(e)");

const calculateWeightedGrade = (subject) => {
  return ((subject.note1 + subject.note2 + subject.exam) * subject.coefficient) / 3;
};
const calculateTotals = computed(() => {
  // Calcul pour les matières littéraires
  const literaryTotal = literarySubjects.value.reduce(
    (acc, subject) => {
      const weightedGrade = calculateWeightedGrade(subject);
      return {
        coefficientSum: acc.coefficientSum + subject.coefficient,
        weightedGradeSum: acc.weightedGradeSum + weightedGrade,
      };
    },
    { coefficientSum: 0, weightedGradeSum: 0 }
  );

  // Calcul pour les matières scientifiques
  const scientificTotal = scientificSubjects.value.reduce(
    (acc, subject) => {
      const weightedGrade = calculateWeightedGrade(subject);
      return {
        coefficientSum: acc.coefficientSum + subject.coefficient,
        weightedGradeSum: acc.weightedGradeSum + weightedGrade,
      };
    },
    { coefficientSum: 0, weightedGradeSum: 0 }
  );

  // Calcul pour les matières optionnelles
  const optionalTotal = optionalSubjects.value.reduce(
    (acc, subject) => {
      const weightedGrade = calculateWeightedGrade(subject);
      return {
        coefficientSum: acc.coefficientSum + subject.coefficient,
        weightedGradeSum: acc.weightedGradeSum + weightedGrade,
      };
    },
    { coefficientSum: 0, weightedGradeSum: 0 }
  );

  // Totaux généraux
  const totalCoefficients =
    literaryTotal.coefficientSum +
    // scientificTotal.coefficientSum +
    optionalTotal.coefficientSum;

  const totalWeightedGrades =
    literaryTotal.weightedGradeSum +
    //  scientificTotal.weightedGradeSum +
    optionalTotal.weightedGradeSum;

  return {
    coefficients: {
      literary: literaryTotal.coefficientSum,
      //   scientific: scientificTotal.coefficientSum,
      optional: optionalTotal.coefficientSum,
      total: totalCoefficients,
    },
    weightedGrades: {
      literary: literaryTotal.weightedGradeSum.toFixed(2),
      //  scientific: scientificTotal.weightedGradeSum.toFixed(2),
      optional: optionalTotal.weightedGradeSum.toFixed(2),
      total: totalWeightedGrades.toFixed(2),
    },
    generalAverage: (totalWeightedGrades / totalCoefficients).toFixed(2),
  };
});

// Calcul pour les données fournies :
const totals = calculateTotals.value;
// Trimester Averages
const firstTrimesterAverage = ref(12.6);
const secondTrimesterAverage = ref(null);
const thirdTrimesterAverage = ref("-");
const Allaverage = computed(() => {
  return (
    (firstTrimesterAverage.value +
      secondTrimesterAverage.value +
      thirdTrimesterAverage.value) /
    3
  ).toFixed(2);
});
// Observations
const observations = ref(
  "L'élève a montré une amélioration constante tout au long de l'année. Continuez vos efforts !"
);

const REMARQUES = [
  "Excellent",
  "Très bien",
  "Bien",
  "Assez bien",
  "Passable",
  "Insuffisant",
];

const getRemarks = (average) => {
  if (average >= 18) return "Excellent";
  if (average >= 15) return "Très bien";
  if (average >= 13) return "Bien";
  if (average >= 11.9) return "Assez bien";
  if (average >= 9.5) return "Passable";
  return "Insuffisant";
};
// Class Teacher
const classTeacher = ref({
  name: "",
});
</script>

<style scoped>
@media print {
  @page {
    size: A4;
    margin: 1cm;
  }
  body {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
}
</style>
