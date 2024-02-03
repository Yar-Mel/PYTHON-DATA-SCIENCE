# Домашнє завдання

Зробіть `summary` нижчевказаного тексту використовуючи бібліотеки для `NLP`: `nltk` та `SpaCy`

    The Orbiter Discovery, OV-103, is considered eligible for listing in the National Register of Historic Places (NRHP) in the context of the U.S. Space Shuttle Program (1969-2011) under Criterion A in the areas of Space Exploration and Transportation and under Criterion C in the area of Engineering. Because it has achieved significance within the past fifty years, Criteria Consideration G applies. Under Criterion A, Discovery is significant as the oldest of the three extant orbiter vehicles constructed for the Space Shuttle Program (SSP), the longest running American space program to date; she was the third of five orbiters built by NASA. Unlike the Mercury, Gemini, and Apollo programs, the SSP’s emphasis was on cost effectiveness and reusability, and eventually the construction of a space station. Including her maiden voyage (launched August 30, 1984), Discovery flew to space thirty-nine times, more than any of the other four orbiters; she was also the first orbiter to fly twenty missions. She had the honor of being chosen as the Return to Flight vehicle after both the Challenger and Columbia accidents. Discovery was the first shuttle to fly with the redesigned SRBs, a result of the Challenger accident, and the first shuttle to fly with the Phase II and Block I SSME. Discovery also carried the Hubble Space Telescope to orbit and performed two of the five servicing missions to the observatory. She flew the first and last dedicated Department of Defense (DoD) missions, as well as the first unclassified defense-related mission. In addition, Discovery was vital to the construction of the International Space Station (ISS); she flew thirteen of the thirty-seven total missions flown to the station by a U.S. Space Shuttle. She was the first orbiter to dock to the ISS, and the first to perform an exchange of a resident crew. Under Criterion C, Discovery is significant as a feat of engineering. According to Wayne Hale, a flight director from Johnson Space Center, the Space Shuttle orbiter represents a “huge technological leap from expendable rockets and capsules to a reusable, winged, hypersonic, cargo-carrying spacecraft.” Although her base structure followed a conventional aircraft design, she used advanced materials that both minimized her weight for cargo-carrying purposes and featured low thermal expansion ratios, which provided a stable base for her Thermal Protection System (TPS) materials. The Space Shuttle orbiter also featured the first reusable TPS; all previous spaceflight vehicles had a single-use, ablative heat shield. Other notable engineering achievements of the orbiter included the first reusable orbital propulsion system, and the first two-fault-tolerant Integrated Avionics System. As Hale stated, the Space Shuttle remains “the largest, fastest, winged hypersonic aircraft in history,” having regularly flown at twenty-five times the speed of sound.

### Підказка

Перш за все, ми повинні імпортувати необхідні бібліотеки. Для `SpaCy` це можна зробити за допомогою команди:

    import spacy

Зауважте, що для `NLTK` можливо потрібно завантажити додаткові дані, наприклад, список стоп-слів або токенізатори.

    import nltk
    from nltk.corpus import stopwords
    from nltk.tokenize import word_tokenize, sent_tokenize

Перш ніж почати роботу з `SpaCy`, необхідно завантажити потрібну мовну модель. Наприклад, для англійської мови ми можемо завантажити модель `"en_core_web_sm"`:

    nlp = spacy.load('en_core_web_sm')

### Підготовка тексту

Перш ніж почати створювати `text summary`, текст потрібно підготувати. Це включає в себе видалення непотрібних символів, токенізацію (розбиття тексту на окремі слова або речення), видалення стоп-слів (слова, які не несуть суттєвої інформації) і, за необхідності, інші обробки тексту, такі як стемінг або лематизація.

### Текст для обробки

    text = "This is an example sentence for tokenization and lemmatization."

### Токенізація

    doc = nlp(text)
    tokens = [token.text for token in doc]
    print(tokens)

`NLTK`також надає розширені функції для обробки тексту. За допомогою методів `NLTK`, таких як `word_tokenize`, `sent_tokenize` або `stopwords`, ми можемо отримати токенизовані слова та речення, а також список стоп-слів.

    tokens = word_tokenize(text)
    sentences = sent_tokenize(text)
    stop_words = set(stopwords.words('english'))

І також не забуваємо про знаки пунктуації

`punctuation = punctuation + '\n'`

Також можна порахувати частоту появи тих чи інших слів у тексті(але варто пам'ятати, що це треба робити, вже виключивши усі знаки пунктуації)

    word_frequencies = {}
    for word in doc:
      if word.text.lower() not in stop_words:
        if word.text.lower() not in punctuation:
          if word.text not in word_frequencies.keys():
            word_frequencies[word.text] = 1
          else:
            word_frequencies[word.text] += 1

Коли ми вже маємо підготовлений текст та використали `SpaCy` або `NLTK` для отримання необхідної інформації, ми можемо створити текстове резюме. Це можна зробити, наприклад, шляхом виділення найважливіших речень з тексту, враховуючи їх вагу або частоту вживання певних слів.

### Бібліотека `heapq`

Бібліотека `heapq` є частиною стандартної бібліотеки `Python` і надає функціонал для роботи зі структурами даних під назвою `heap`. Один з імпортованих об’єктів у цій бібліотеці - `nlargest` - є функцією, яка дозволяє знаходити найбільші елементи з ітерабельного об’єкту.

    from heapq import nlargest

Функція `nlargest(n, iterable, key=None)` приймає три аргументи:

- `n` - це кількість найбільших елементів, які ви хочете отримати
- `iterable` - це ітерабельний об’єкт, з якого ви хочете вибрати найбільші елементи
- `key` (необов’язковий) - це функція, яка визначає, за яким ключем відбувається порівняння елементів (наприклад, `key=str.lower`)
- Функція `nlargest` повертає список з `n` найбільших елементів з `iterable`. Ці елементи будуть впорядковані у порядку спадання. Якщо n більше довжини `iterable`, то функція поверне весь `iterable` у відсортованому порядку.

Отже, імпортований `from heapq import nlargest` дозволяє використовувати функцію `nlargest` для знаходження найбільших елементів з довільного ітерабельного об’єкту.

    select_length = int(len(sentence_tokens))
    summary = nlargest(select_length, sentence_scores, key = sentence_scores.get)
    summary

У даному випадку, функція `nlargest` використовується для знаходження в `select_length` найбільших елементів зі словника `sentence_scores`. Ключі словника представляють речення, а значення - їхні оцінки або ваги. Аргумент key заданий як `sentence_scores.get`, що означає, що функція `get` використовується для порівняння елементів. У даному випадку, вона повертає значення (оцінку) для кожного речення, яке використовується як критерій для порівняння. Отже, змінна summary міститиме список `select_length` найкращих речень зі словника `sentence_scores` у порядку спадання оцінок.
