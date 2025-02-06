<template>
  <div class="min-h-screen bg-gray-100 p-8 print:p-0 print:bg-white">
    <div class="max-w-4xl mx-auto bg-white shadow-lg p-8 print:shadow-none print:p-4">
      <h1 class="text-3xl font-bold mb-6">Générateur de Bulletin Scolaire</h1>

      <!-- Formulaire d'entrée -->
      <form
        v-if="!showBulletin"
        @submit.prevent="generateBulletin"
        class="space-y-6 mb-8"
      >
        <!-- Informations de l'école -->
        <div>
          <h2 class="text-xl font-semibold mb-2">Informations de l'école</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="schoolName" class="block text-sm font-medium text-gray-700"
                >Nom de l'école</label
              >
              <input
                v-model="schoolInfo.name"
                id="schoolName"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="schoolAddress" class="block text-sm font-medium text-gray-700"
                >Adresse</label
              >
              <input
                v-model="schoolInfo.address"
                id="schoolAddress"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="schoolPhone" class="block text-sm font-medium text-gray-700"
                >Téléphone</label
              >
              <input
                v-model="schoolInfo.phone"
                id="schoolPhone"
                type="tel"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
          </div>
        </div>

        <!-- Informations de l'élève -->
        <div>
          <h2 class="text-xl font-semibold mb-2">Informations de l'élève</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="studentId" class="block text-sm font-medium text-gray-700"
                >Matricule</label
              >
              <input
                v-model="student.id"
                id="studentId"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="studentName" class="block text-sm font-medium text-gray-700"
                >Nom & Prénom</label
              >
              <input
                v-model="student.name"
                id="studentName"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="studentClass" class="block text-sm font-medium text-gray-700"
                >Classe</label
              >
              <input
                v-model="student.class"
                id="studentClass"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="academicYear" class="block text-sm font-medium text-gray-700"
                >Année Académique</label
              >
              <input
                v-model="student.academicYear"
                id="academicYear"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
          </div>
        </div>

        <!-- Matières et notes -->
        <div>
          <h2 class="text-xl font-semibold mb-2">Matières et notes</h2>
          <div v-for="(category, categoryIndex) in subjects" :key="categoryIndex">
            <h3 class="text-lg font-medium mb-2">{{ category.name }}</h3>
            <div
              v-for="(subject, subjectIndex) in category.subjects"
              :key="subjectIndex"
              class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4"
            >
              <div>
                <label
                  :for="`subject-${categoryIndex}-${subjectIndex}`"
                  class="block text-sm font-medium text-gray-700"
                  >Matière</label
                >
                <input
                  v-model="subject.name"
                  :id="`subject-${categoryIndex}-${subjectIndex}`"
                  type="text"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                />
              </div>
              <div>
                <label
                  :for="`note1-${categoryIndex}-${subjectIndex}`"
                  class="block text-sm font-medium text-gray-700"
                  >Note 1</label
                >
                <input
                  v-model.number="subject.note1"
                  :id="`note1-${categoryIndex}-${subjectIndex}`"
                  type="number"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                />
              </div>
              <div>
                <label
                  :for="`note2-${categoryIndex}-${subjectIndex}`"
                  class="block text-sm font-medium text-gray-700"
                  >Note 2</label
                >
                <input
                  v-model.number="subject.note2"
                  :id="`note2-${categoryIndex}-${subjectIndex}`"
                  type="number"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                />
              </div>
              <div>
                <label
                  :for="`exam-${categoryIndex}-${subjectIndex}`"
                  class="block text-sm font-medium text-gray-700"
                  >Composition</label
                >
                <input
                  v-model.number="subject.exam"
                  :id="`exam-${categoryIndex}-${subjectIndex}`"
                  type="number"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                />
              </div>
              <div>
                <label
                  :for="`coefficient-${categoryIndex}-${subjectIndex}`"
                  class="block text-sm font-medium text-gray-700"
                  >Coefficient</label
                >
                <input
                  v-model.number="subject.coefficient"
                  :id="`coefficient-${categoryIndex}-${subjectIndex}`"
                  type="number"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                />
              </div>
              <div>
                <label
                  :for="`teacher-${categoryIndex}-${subjectIndex}`"
                  class="block text-sm font-medium text-gray-700"
                  >Professeur</label
                >
                <input
                  v-model="subject.teacher"
                  :id="`teacher-${categoryIndex}-${subjectIndex}`"
                  type="text"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                />
              </div>
            </div>
            <button
              @click="addSubject(categoryIndex)"
              type="button"
              class="mb-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
            >
              Ajouter une matière
            </button>
          </div>
        </div>

        <!-- Autres informations -->
        <div>
          <h2 class="text-xl font-semibold mb-2">Autres informations</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="classSize" class="block text-sm font-medium text-gray-700"
                >Effectif de la classe</label
              >
              <input
                v-model.number="classSize"
                id="classSize"
                type="number"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="decision" class="block text-sm font-medium text-gray-700"
                >Décision</label
              >
              <input
                v-model="decision"
                id="decision"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label
                for="firstTrimesterAverage"
                class="block text-sm font-medium text-gray-700"
                >Moyenne 1er Trimestre</label
              >
              <input
                v-model="firstTrimesterAverage"
                id="firstTrimesterAverage"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label
                for="secondTrimesterAverage"
                class="block text-sm font-medium text-gray-700"
                >Moyenne 2ème Trimestre</label
              >
              <input
                v-model="secondTrimesterAverage"
                id="secondTrimesterAverage"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label
                for="thirdTrimesterAverage"
                class="block text-sm font-medium text-gray-700"
                >Moyenne 3ème Trimestre</label
              >
              <input
                v-model="thirdTrimesterAverage"
                id="thirdTrimesterAverage"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
            <div>
              <label for="observations" class="block text-sm font-medium text-gray-700"
                >Observations générales</label
              >
              <textarea
                v-model="observations"
                id="observations"
                rows="3"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              ></textarea>
            </div>
            <div>
              <label
                for="classTeacherName"
                class="block text-sm font-medium text-gray-700"
                >Nom du titulaire de classe</label
              >
              <input
                v-model="classTeacher.name"
                id="classTeacherName"
                type="text"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
              />
            </div>
          </div>
        </div>

        <button
          type="submit"
          class="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50"
        >
          Générer le bulletin
        </button>
      </form>

      <!-- Bulletin généré -->
      <div v-if="showBulletin" class="mb-8 print:mb-0" id="bulletin">
        <h1 class="text-3xl font-bold uppercase mb-4 text-center">BULLETIN</h1>
        <div class="text-center mb-6">
          <h2 class="text-xl font-bold uppercase mb-2">{{ schoolInfo.name }}</h2>
          <p class="text-sm">{{ schoolInfo.address }}</p>
          <p class="text-sm">Tel: {{ schoolInfo.phone }}</p>
        </div>

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

        <div class="overflow-x-auto mb-6">
          <table class="w-full text-sm border-collapse">
            <thead>
              <tr class="bg-gray-100">
                <th class="border p-2 text-left">MATIERES</th>
                <th class="border p-2">NOTE 1</th>
                <th class="border p-2">NOTE 2</th>
                <th class="border p-2">MOY CLASSE</th>
                <th class="border p-2">COMPO</th>
                <th class="border p-2">NOTE MOY</th>
                <th class="border p-2">COEF</th>
                <th class="border p-2">NOTE COEF</th>
                <th class="border p-2">RANG</th>
                <th class="border p-2">PROFESSEUR</th>
                <th class="border p-2">REMARQUES</th>
              </tr>
            </thead>
            <tbody>
              <template
                v-for="(category, categoryIndex) in subjects"
                :key="categoryIndex"
              >
                <tr class="bg-gray-50">
                  <td colspan="11" class="border p-2 font-bold">{{ category.name }}</td>
                </tr>
                <tr v-for="subject in category.subjects" :key="subject.name">
                  <td class="border p-2">{{ subject.name }}</td>
                  <td class="border p-2 text-center">{{ subject.note1 }}</td>
                  <td class="border p-2 text-center">{{ subject.note2 }}</td>
                  <template>
                    <div class="min-h-screen bg-gray-100 p-8 print:p-0 print:bg-white">
                      <div
                        class="max-w-4xl mx-auto bg-white shadow-lg p-8 print:shadow-none print:p-4"
                      >
                        <h1 class="text-3xl font-bold mb-6">
                          Générateur de Bulletin Scolaire
                        </h1>

                        <!-- Formulaire d'entrée -->
                        <form
                          v-if="!showBulletin"
                          @submit.prevent="generateBulletin"
                          class="space-y-6 mb-8"
                        >
                          <!-- Informations de l'école -->
                          <div>
                            <h2 class="text-xl font-semibold mb-2">
                              Informations de l'école
                            </h2>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                              <div>
                                <label
                                  for="schoolName"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Nom de l'école</label
                                >
                                <input
                                  v-model="schoolInfo.name"
                                  id="schoolName"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="schoolAddress"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Adresse</label
                                >
                                <input
                                  v-model="schoolInfo.address"
                                  id="schoolAddress"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="schoolPhone"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Téléphone</label
                                >
                                <input
                                  v-model="schoolInfo.phone"
                                  id="schoolPhone"
                                  type="tel"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                            </div>
                          </div>

                          <!-- Informations de l'élève -->
                          <div>
                            <h2 class="text-xl font-semibold mb-2">
                              Informations de l'élève
                            </h2>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                              <div>
                                <label
                                  for="studentId"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Matricule</label
                                >
                                <input
                                  v-model="student.id"
                                  id="studentId"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="studentName"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Nom & Prénom</label
                                >
                                <input
                                  v-model="student.name"
                                  id="studentName"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="studentClass"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Classe</label
                                >
                                <input
                                  v-model="student.class"
                                  id="studentClass"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="academicYear"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Année Académique</label
                                >
                                <input
                                  v-model="student.academicYear"
                                  id="academicYear"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                            </div>
                          </div>

                          <!-- Matières et notes -->
                          <div>
                            <h2 class="text-xl font-semibold mb-2">Matières et notes</h2>
                            <div
                              v-for="(category, categoryIndex) in subjects"
                              :key="categoryIndex"
                              class="mb-4"
                            >
                              <h3 class="text-lg font-medium mb-2">
                                {{ category.name }}
                              </h3>
                              <div
                                v-for="(subject, subjectIndex) in category.subjects"
                                :key="subjectIndex"
                                class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4"
                              >
                                <div>
                                  <label
                                    :for="`subject-${categoryIndex}-${subjectIndex}`"
                                    class="block text-sm font-medium text-gray-700 mb-1"
                                    >Matière</label
                                  >
                                  <input
                                    v-model="subject.name"
                                    :id="`subject-${categoryIndex}-${subjectIndex}`"
                                    type="text"
                                    class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                  />
                                </div>
                                <div>
                                  <label
                                    :for="`note1-${categoryIndex}-${subjectIndex}`"
                                    class="block text-sm font-medium text-gray-700 mb-1"
                                    >Note 1</label
                                  >
                                  <input
                                    v-model.number="subject.note1"
                                    :id="`note1-${categoryIndex}-${subjectIndex}`"
                                    type="number"
                                    min="0"
                                    max="20"
                                    step="0.25"
                                    class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                  />
                                </div>
                                <div>
                                  <label
                                    :for="`note2-${categoryIndex}-${subjectIndex}`"
                                    class="block text-sm font-medium text-gray-700 mb-1"
                                    >Note 2</label
                                  >
                                  <input
                                    v-model.number="subject.note2"
                                    :id="`note2-${categoryIndex}-${subjectIndex}`"
                                    type="number"
                                    min="0"
                                    max="20"
                                    step="0.25"
                                    class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                  />
                                </div>
                                <div>
                                  <label
                                    :for="`exam-${categoryIndex}-${subjectIndex}`"
                                    class="block text-sm font-medium text-gray-700 mb-1"
                                    >Composition</label
                                  >
                                  <input
                                    v-model.number="subject.exam"
                                    :id="`exam-${categoryIndex}-${subjectIndex}`"
                                    type="number"
                                    min="0"
                                    max="20"
                                    step="0.25"
                                    class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                  />
                                </div>
                                <div>
                                  <label
                                    :for="`coefficient-${categoryIndex}-${subjectIndex}`"
                                    class="block text-sm font-medium text-gray-700 mb-1"
                                    >Coefficient</label
                                  >
                                  <input
                                    v-model.number="subject.coefficient"
                                    :id="`coefficient-${categoryIndex}-${subjectIndex}`"
                                    type="number"
                                    min="1"
                                    step="1"
                                    class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                  />
                                </div>
                                <div>
                                  <label
                                    :for="`teacher-${categoryIndex}-${subjectIndex}`"
                                    class="block text-sm font-medium text-gray-700 mb-1"
                                    >Professeur</label
                                  >
                                  <input
                                    v-model="subject.teacher"
                                    :id="`teacher-${categoryIndex}-${subjectIndex}`"
                                    type="text"
                                    class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                  />
                                </div>
                              </div>
                              <button
                                @click="addSubject(categoryIndex)"
                                type="button"
                                class="mt-2 px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                              >
                                Ajouter une matière
                              </button>
                            </div>
                          </div>

                          <!-- Autres informations -->
                          <div>
                            <h2 class="text-xl font-semibold mb-2">
                              Autres informations
                            </h2>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                              <div>
                                <label
                                  for="classSize"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Effectif de la classe</label
                                >
                                <input
                                  v-model.number="classSize"
                                  id="classSize"
                                  type="number"
                                  min="1"
                                  step="1"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="decision"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Décision</label
                                >
                                <input
                                  v-model="decision"
                                  id="decision"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="firstTrimesterAverage"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Moyenne 1er Trimestre</label
                                >
                                <input
                                  v-model="firstTrimesterAverage"
                                  id="firstTrimesterAverage"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="secondTrimesterAverage"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Moyenne 2ème Trimestre</label
                                >
                                <input
                                  v-model="secondTrimesterAverage"
                                  id="secondTrimesterAverage"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="thirdTrimesterAverage"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Moyenne 3ème Trimestre</label
                                >
                                <input
                                  v-model="thirdTrimesterAverage"
                                  id="thirdTrimesterAverage"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                              <div>
                                <label
                                  for="observations"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Observations générales</label
                                >
                                <textarea
                                  v-model="observations"
                                  id="observations"
                                  rows="3"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                ></textarea>
                              </div>
                              <div>
                                <label
                                  for="classTeacherName"
                                  class="block text-sm font-medium text-gray-700 mb-1"
                                  >Nom du titulaire de classe</label
                                >
                                <input
                                  v-model="classTeacher.name"
                                  id="classTeacherName"
                                  type="text"
                                  class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-1 focus:ring-blue-500 focus:border-blue-500"
                                />
                              </div>
                            </div>
                          </div>

                          <button
                            type="submit"
                            class="w-full px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                          >
                            Générer le bulletin
                          </button>
                        </form>

                        <!-- Bulletin généré -->
                        <div v-if="showBulletin" class="mb-8 print:mb-0" id="bulletin">
                          <h1 class="text-3xl font-bold uppercase mb-4 text-center">
                            BULLETIN
                          </h1>
                          <div class="text-center mb-6">
                            <h2 class="text-xl font-bold uppercase mb-2">
                              {{ schoolInfo.name }}
                            </h2>
                            <p class="text-sm">{{ schoolInfo.address }}</p>
                            <p class="text-sm">Tel: {{ schoolInfo.phone }}</p>
                          </div>

                          <div class="grid grid-cols-2 gap-4 mb-6 text-sm">
                            <div>
                              <p>
                                <span class="font-bold">Matricule:</span> {{ student.id }}
                              </p>
                              <p>
                                <span class="font-bold">Nom & Prénom:</span>
                                {{ student.name }}
                              </p>
                            </div>
                            <div>
                              <p>
                                <span class="font-bold">Classe:</span> {{ student.class }}
                              </p>
                              <p>
                                <span class="font-bold">Année Académique:</span>
                                {{ student.academicYear }}
                              </p>
                            </div>
                          </div>

                          <div class="overflow-x-auto mb-6">
                            <table class="w-full text-sm border-collapse">
                              <thead>
                                <tr class="bg-gray-100">
                                  <th class="border p-2 text-left">MATIERES</th>
                                  <th class="border p-2">NOTE 1</th>
                                  <th class="border p-2">NOTE 2</th>
                                  <th class="border p-2">MOY CLASSE</th>
                                  <th class="border p-2">COMPO</th>
                                  <th class="border p-2">NOTE MOY</th>
                                  <th class="border p-2">COEF</th>
                                  <th class="border p-2">NOTE COEF</th>
                                  <th class="border p-2">RANG</th>
                                  <th class="border p-2">PROFESSEUR</th>
                                  <th class="border p-2">REMARQUES</th>
                                </tr>
                              </thead>
                              <tbody>
                                <template
                                  v-for="(category, categoryIndex) in subjects"
                                  :key="categoryIndex"
                                >
                                  <tr class="bg-gray-50">
                                    <td colspan="11" class="border p-2 font-bold">
                                      {{ category.name }}
                                    </td>
                                  </tr>
                                  <tr
                                    v-for="subject in category.subjects"
                                    :key="subject.name"
                                  >
                                    <td class="border p-2">{{ subject.name }}</td>
                                    <td class="border p-2 text-center">
                                      {{ subject.note1 }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ subject.note2 }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ calculateAverage(subject.note1, subject.note2) }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ subject.exam }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ calculateFinalAverage(subject) }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ subject.coefficient }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ calculateWeightedGrade(subject) }}
                                    </td>
                                    <td class="border p-2 text-center">
                                      {{ calculateRank(subject, category.subjects) }}
                                    </td>
                                    <td class="border p-2">{{ subject.teacher }}</td>
                                    <td class="border p-2">
                                      {{ getRemarks(calculateFinalAverage(subject)) }}
                                    </td>
                                  </tr>
                                </template>
                              </tbody>
                            </table>
                          </div>

                          <div class="mb-6 text-sm">
                            <div class="grid grid-cols-2 gap-4">
                              <div>
                                <p>
                                  <span class="font-bold">Total des coefficients:</span>
                                  {{ totalCoefficients }}
                                </p>
                              </div>
                              <div>
                                <p>
                                  <span class="font-bold"
                                    >Total des notes coefficientées:</span
                                  >
                                  {{ totalWeightedGrades }}
                                </p>
                              </div>
                            </div>
                          </div>

                          <div class="mb-6 text-sm">
                            <div class="grid grid-cols-2 gap-4">
                              <div>
                                <p>
                                  <span class="font-bold">Moyenne Générale:</span>
                                  {{ generalAverage }}
                                </p>
                                <p>
                                  <span class="font-bold">Rang:</span>
                                  {{ calculateOverallRank() }}
                                </p>
                              </div>
                              <div>
                                <p>
                                  <span class="font-bold">Effectif:</span> {{ classSize }}
                                </p>
                                <p>
                                  <span class="font-bold">Décision:</span> {{ decision }}
                                </p>
                              </div>
                            </div>
                          </div>

                          <div class="mb-6 text-sm">
                            <h3 class="font-bold mb-2">Moyennes Trimestrielles</h3>
                            <div class="grid grid-cols-3 gap-4">
                              <div>
                                <p>
                                  <span class="font-bold">1er Trimestre:</span>
                                  {{ firstTrimesterAverage }}
                                </p>
                              </div>
                              <div>
                                <p>
                                  <span class="font-bold">2ème Trimestre:</span>
                                  {{ secondTrimesterAverage }}
                                </p>
                              </div>
                              <div>
                                <p>
                                  <span class="font-bold">3ème Trimestre:</span>
                                  {{ thirdTrimesterAverage }}
                                </p>
                              </div>
                            </div>
                          </div>

                          <div class="mb-6 text-sm">
                            <h3 class="font-bold mb-2">Observations Générales</h3>
                            <p>{{ observations }}</p>
                          </div>

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
                                <p>Date: {{ new Date().toLocaleDateString() }}</p>
                                <p class="mt-4">Le Directeur</p>
                                <div class="mt-2 h-10 border-b border-black"></div>
                              </div>
                            </div>
                          </div>
                        </div>

                        <!-- Bouton d'impression -->
                        <button
                          v-if="showBulletin"
                          @click="printBulletin"
                          class="mt-4 px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                        >
                          Imprimer le bulletin
                        </button>
                      </div>
                    </div>
                  </template>
                </tr></template
              >
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const showBulletin = ref(false);

const schoolInfo = ref({
  name: "",
  address: "",
  phone: "",
});

const student = ref({
  id: "",
  name: "",
  class: "",
  academicYear: "",
});

const subjects = ref([
  {
    name: "Matières littéraires",
    subjects: [{ name: "", note1: 0, note2: 0, exam: 0, coefficient: 1, teacher: "" }],
  },
  {
    name: "Matières scientifiques",
    subjects: [{ name: "", note1: 0, note2: 0, exam: 0, coefficient: 1, teacher: "" }],
  },
  {
    name: "Matières facultatives",
    subjects: [{ name: "", note1: 0, note2: 0, exam: 0, coefficient: 1, teacher: "" }],
  },
]);

const classSize = ref(0);
const decision = ref("");
const firstTrimesterAverage = ref("");
const secondTrimesterAverage = ref("");
const thirdTrimesterAverage = ref("");
const observations = ref("");
const classTeacher = ref({ name: "" });

const addSubject = (categoryIndex) => {
  subjects.value[categoryIndex].subjects.push({
    name: "",
    note1: 0,
    note2: 0,
    exam: 0,
    coefficient: 1,
    teacher: "",
  });
};

const calculateAverage = (note1, note2) => {
  return ((parseFloat(note1) + parseFloat(note2)) / 2).toFixed(2);
};

const calculateFinalAverage = (subject) => {
  const classAverage = calculateAverage(subject.note1, subject.note2);
  return ((parseFloat(classAverage) + parseFloat(subject.exam)) / 2).toFixed(2);
};

const calculateWeightedGrade = (subject) => {
  return (
    parseFloat(calculateFinalAverage(subject)) * parseFloat(subject.coefficient)
  ).toFixed(2);
};

const calculateRank = (subject, subjectList) => {
  const sortedSubjects = [...subjectList].sort(
    (a, b) => parseFloat(calculateFinalAverage(b)) - parseFloat(calculateFinalAverage(a))
  );
  return sortedSubjects.findIndex((s) => s.name === subject.name) + 1;
};

const getRemarks = (average) => {
  if (average >= 16) return "Excellent";
  if (average >= 14) return "Très bien";
  if (average >= 12) return "Bien";
  if (average >= 10) return "Assez bien";
  if (average >= 8) return "Passable";
  return "Insuffisant";
};

const totalCoefficients = computed(() => {
  return subjects.value.reduce((total, category) => {
    return (
      total +
      category.subjects.reduce(
        (catTotal, subject) => catTotal + parseFloat(subject.coefficient),
        0
      )
    );
  }, 0);
});

const totalWeightedGrades = computed(() => {
  return subjects.value.reduce((total, category) => {
    return (
      total +
      category.subjects.reduce(
        (catTotal, subject) => catTotal + parseFloat(calculateWeightedGrade(subject)),
        0
      )
    );
  }, 0);
});

const generalAverage = computed(() => {
  return (totalWeightedGrades.value / totalCoefficients.value).toFixed(2);
});

const calculateOverallRank = () => {
  // Ceci est un espace réservé. Dans une application réelle, vous calculeriez le rang en fonction des performances de toute la classe.
  return "4e";
};

const generateBulletin = () => {
  showBulletin.value = true;
};

const printBulletin = () => {
  window.print();
};
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
