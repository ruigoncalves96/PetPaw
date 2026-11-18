# PetPaw — Pet Owner Survey (Tally build sheet)

*Phase 0 · Validation. Goal: quantify pains and species mix, and recruit 15–20 interviewees.*
*Target length: **6–8 minutes**, about 25–30 questions per respondent (conditional logic hides the rest).*

---

## 0. How to use this document

1. **Build the English form first** in Tally, block by block, following the tables below. Each question shows the **Tally block type**, whether it's **required**, and any **logic**.
2. **Duplicate the form** and replace the text with the **PT-PT** wording given under each question. Keep the same order and options so both forms produce the same columns.
3. Connect **both forms to the same Google Sheet** (Integrations → Google Sheets) in two tabs, `EN` and `PT`, then merge them (see §9).
4. Add the **hidden fields** (§1) and share links with `?src=...` so you know where each answer came from.
5. **Pilot with 5 people** before you publish (§10).

### Rules that keep the data easy to analyse
- **No commas inside Checkboxes (multi-select) option labels.** Tally sends multi-select answers to Sheets as *comma-separated text*, so a comma inside a label would break counting. This document uses `/` instead in those questions. Single-choice questions (Multiple choice / Dropdown) can use commas safely.
- **Option labels must be unique words.** Don't use both "Dog" and "Dog walker" as options in one question, because `COUNTIF(..."*Dog*")` would match both.
- **Keep option order fixed** across EN and PT (same position = same meaning). Use the **Codebook IDs** (§9) as your column names in the analysis sheet.
- **Keep the question IDs (A1, B2…) out of the visible title.** They're for you only. Tally lets you write a short internal title per block if you want it.

---

## 1. Form settings

| Setting | Value |
|---|---|
| Form title | EN: *Help us build the pet care app you actually need* · PT: *Ajude-nos a criar a app de cuidados para animais de que realmente precisa* |
| Layout | One question group per page (use **Page breaks** between sections A–F) |
| Progress bar | On |
| Hidden fields | `lang` (`en` / `pt`), `src` (where the link was shared: `reddit_reptiles`, `fb_caes_pt`, `vet_clinic_x`, `friends`…) |
| Response limit | None |
| Close date | Optional: 4–6 weeks after launch |
| Branding | Your logo/colour once the name is final. Tally's free plan shows a small "Made with Tally" badge, which is fine. |
| Notifications | Email on new submission: off (you'll get many). Check the Sheet instead. |

Example share link: `https://tally.so/r/XXXX?lang=en&src=reddit_reptiles`

---

## 2. Intro page (Text block, no questions)

**EN**
> 🐾 **Hi! We're building a free app to help people look after *any* animal — dogs, cats, rabbits, birds, reptiles, fish, horses and more.**
> Before we write a single line of code, we want to understand how you care for your animals today and what gets in the way.
> It takes about **6 minutes**. Your answers are anonymous unless you choose to leave your email at the end.
> Thank you! 🙏

**PT**
> 🐾 **Olá! Estamos a criar uma app gratuita para ajudar a cuidar de *qualquer* animal — cães, gatos, coelhos, aves, répteis, peixes, cavalos e muito mais.**
> Antes de escrevermos uma única linha de código, queremos perceber como cuida dos seus animais hoje e o que mais lhe dificulta a vida.
> Demora cerca de **6 minutos**. As respostas são anónimas, a não ser que decida deixar o seu email no final.
> Obrigado! 🙏

---

## 3. Section A — About you and your animals

### A1 · Pet status
- **Block:** Multiple choice (single) · **Required**
- **EN:** Do you currently have any pets or animals in your care?
- **PT:** Atualmente tem animais de companhia ou outros animais a seu cuidado?

| # | EN | PT |
|---|---|---|
| 1 | Yes | Sim |
| 2 | Not now, but I had one in the last 2 years | Agora não, mas tive nos últimos 2 anos |
| 3 | No, but I'm planning to get one | Não, mas estou a pensar ter |
| 4 | No | Não |

- **Logic:** If A1 is **"No"** → *Jump to* **Section F** (follow-up page) so they can still join the waitlist. If A1 is option 2 or 3 → continue (answer about past/planned pets).

### A2 · Species
- **Block:** Checkboxes (multi) · **Required** · add **"Other"** option with text
- **EN:** Which animals live with you (or did / will)? *Select all that apply.*
- **PT:** Que animais vivem consigo (ou viveram / vão viver)? *Selecione todos os que se aplicam.*

| # | EN | PT |
|---|---|---|
| 1 | Dog | Cão |
| 2 | Cat | Gato |
| 3 | Rabbit / guinea pig / hamster / other small mammal | Coelho / porquinho-da-índia / hamster / outro pequeno mamífero |
| 4 | Bird | Ave |
| 5 | Reptile or amphibian | Réptil ou anfíbio |
| 6 | Fish / aquarium / pond | Peixes / aquário / lago |
| 7 | Horse / pony / donkey | Cavalo / pónei / burro |
| 8 | Farm animals (chickens / goats / pigs…) | Animais de quinta (galinhas / cabras / porcos…) |
| 9 | Invertebrates (tarantulas / insects / snails…) | Invertebrados (tarântulas / insetos / caracóis…) |
| 10 | Other | Outro |

> ⚠️ Rows 8 and 9 use "/" inside brackets, so there's still no comma. Keep it that way.

### A3 · Number of animals
- **Block:** Multiple choice · **Required**
- **EN:** How many animals in total?
- **PT:** Quantos animais no total?

| EN | PT |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3–5 | 3–5 |
| 6–10 | 6–10 |
| More than 10 | Mais de 10 |

### A4 · Who cares for them
- **Block:** Checkboxes · **Required**
- **EN:** Who helps take care of them? *Select all that apply.*
- **PT:** Quem ajuda a cuidar deles? *Selecione todas as opções aplicáveis.*

| # | EN | PT |
|---|---|---|
| 1 | Only me | Só eu |
| 2 | My partner / family / housemates | Companheiro(a) / família / pessoas com quem vivo |
| 3 | Children in the household | Crianças da casa |
| 4 | A paid sitter / walker / daycare | Um pet sitter / passeador / creche paga |
| 5 | Friends or neighbours when I'm away | Amigos ou vizinhos quando estou fora |
| 6 | A stable / livery / boarding facility | Uma coudelaria / estábulo / hotel para animais |

- No logic. Analysis uses this question to define the *shared care* segment.

### A5 · Your role
- **Block:** Checkboxes · **Required**
- **EN:** Which of these describe you? *Select all that apply.*
- **PT:** Quais destas opções o descrevem? *Selecione todas as que se aplicam.*

| # | EN | PT |
|---|---|---|
| 1 | Pet owner / keeper | Tutor / dono de animais |
| 2 | Breeder | Criador |
| 3 | Shelter / rescue volunteer or staff | Voluntário ou funcionário de associação / abrigo |
| 4 | Vet / vet nurse / clinic staff | Veterinário / enfermeiro veterinário / staff de clínica |
| 5 | Pet sitter / walker / groomer / trainer | Pet sitter / passeador / tosquiador / treinador |
| 6 | Other | Outro |

### A6 · Country
- **Block:** Dropdown · **Required**
- **EN:** Where do you live?
- **PT:** Onde vive?
- **Options (same in both, translate labels in PT form):** Portugal · Spain / Espanha · United Kingdom / Reino Unido · Ireland / Irlanda · France / França · Germany / Alemanha · Italy / Itália · Netherlands / Países Baixos · Other EU country / Outro país da UE · Brazil / Brasil · United States / Estados Unidos · Canada / Canadá · Other / Outro

### A7 · Age
- **Block:** Multiple choice · **Required**
- **EN:** How old are you?
- **PT:** Que idade tem?
- **Options:** Under 18 / Menos de 18 · 18–24 · 25–34 · 35–44 · 45–54 · 55–64 · 65+ · Prefer not to say / Prefiro não dizer

> 🔞 If A7 = "Under 18", jump to the Thank-you page (don't collect email from minors, for GDPR).

**— Page break —**

---

## 4. Section B — How you manage care today

### B1 · Current tools
- **Block:** Checkboxes · **Required**
- **EN:** How do you keep track of your animals' health and care today (vaccines, vet visits, medications, feeding…)? *Select all that apply.*
- **PT:** Como regista hoje a saúde e os cuidados dos seus animais (vacinas, consultas, medicação, alimentação…)? *Selecione todas as opções aplicáveis.*

| # | EN | PT |
|---|---|---|
| 1 | I keep it in my head | De memória |
| 2 | Paper booklet / vaccine card / passport | Boletim de vacinas / caderneta / passaporte em papel |
| 3 | Notes app on my phone | App de notas no telemóvel |
| 4 | Spreadsheet | Folha de cálculo (Excel / Sheets) |
| 5 | Calendar reminders | Lembretes no calendário |
| 6 | A pet care app | Uma app de cuidados para animais |
| 7 | My vet's app / emails / SMS | App / emails / SMS da clínica veterinária |
| 8 | Notebook / paper log | Caderno / registo em papel |
| 9 | I don't really track it | Não registo |
| 10 | Other | Outro |

### B2 · Which app
- **Block:** Short answer · Optional
- **Logic:** **Show only if** B1 contains "A pet care app" / "Uma app de cuidados para animais"
- **EN:** Which app(s) do you use?
- **PT:** Que app(s) utiliza?

### B3 · Pain events (last 12 months)
- **Block:** Checkboxes · **Required**
- **EN:** In the last 12 months, did any of these happen to you? *Select all that apply.*
- **PT:** Nos últimos 12 meses, aconteceu-lhe alguma destas situações? *Selecione todas as que se aplicam.*

| # | EN | PT |
|---|---|---|
| 1 | A vaccine / deworming / flea treatment was late or missed | Uma vacina / desparasitação / tratamento antipulgas ficou atrasado ou foi esquecido |
| 2 | A medication dose was forgotten | Esqueci-me de uma dose de medicação |
| 3 | Couldn't find a record or document when I needed it | Não encontrei um registo ou documento quando precisei |
| 4 | An animal was fed or medicated twice by different people | Um animal foi alimentado ou medicado duas vezes por pessoas diferentes |
| 5 | Didn't remember when something last happened (last shed / nail trim / water change…) | Não me lembrava de quando algo aconteceu pela última vez (última muda / corte de unhas / mudança de água…) |
| 6 | A sitter or family member didn't have the info they needed | Um pet sitter ou familiar não tinha a informação de que precisava |
| 7 | The vet asked something about history that I couldn't answer | O veterinário perguntou algo sobre o historial e não soube responder |
| 8 | Problems with travel documents / passport / vaccine validity | Problemas com documentos de viagem / passaporte / validade de vacinas |
| 9 | Noticed a health problem late (weight loss / not eating…) | Detetei um problema de saúde tarde (perda de peso / falta de apetite…) |
| 10 | None of these | Nenhuma destas |

### B4 · The story (open)
- **Block:** Long answer · Optional (but this is the most valuable answer, so keep it visible)
- **Logic:** **Hide if** B3 = "None of these" only
- **EN:** Tell us about the last time one of these happened. What happened, and what did you do?
- **PT:** Conte-nos a última vez que uma destas situações aconteceu. O que se passou e o que fez?
- Placeholder EN: *e.g. "My partner and I both gave the dog his pill…"* · PT: *ex.: "Eu e o meu companheiro demos os dois o comprimido ao cão…"*

### B5 · Vet frequency
- **Block:** Multiple choice · **Required**
- **EN:** On average, how often does each animal go to the vet per year?
- **PT:** Em média, quantas vezes por ano vai cada animal ao veterinário?
- **Options:** Never / Nunca · Once / 1 vez · 2–3 times / 2–3 vezes · 4 or more / 4 ou mais · It varies a lot / Varia muito

### B6 · Monthly spend
- **Block:** Multiple choice · **Required**
- **EN:** Roughly how much do you spend on your animals per month, excluding emergencies?
- **PT:** Aproximadamente quanto gasta por mês com os seus animais, excluindo emergências?
- **Options:** Less than €25 / Menos de 25€ · €25–50 / 25–50€ · €50–100 / 50–100€ · €100–250 / 100–250€ · More than €250 / Mais de 250€ · Prefer not to say / Prefiro não dizer

**— Page break —**

---

## 5. Section C — Species-specific care (conditional)

**Put all of section C on one page.** Each block below is **hidden by default** and shown with **Tally logic: "If A2 contains [option] → Show block"**. Respondents only see the blocks for the animals they selected.

Every question here is **Checkboxes, optional**, with an "Other" option.

**Shared wording (adapt per species):**
- **EN:** What do you need to keep track of for your **[dog(s)]**? *Select all that apply.*
- **PT:** O que precisa de acompanhar/registar para o(s) seu(s) **[cão/cães]**? *Selecione todas as opções aplicáveis.*

### C1 · Dog *(show if A2 contains Dog)*
EN · PT
1. Vaccines · Vacinas
2. Deworming / flea / tick treatments · Desparasitação / pulgas / carraças
3. Medications · Medicação
4. Weight · Peso
5. Walks / exercise · Passeios / exercício
6. Food / diet · Alimentação / dieta
7. Grooming / baths · Tosquia / banhos
8. Training / behaviour · Treino / comportamento
9. Vet visits / test results · Consultas / resultados de análises
10. Microchip / passport / registration · Microchip / passaporte / registo (SIAC)
11. Insurance · Seguro
12. Expenses · Despesas

### C2 · Cat *(show if A2 contains Cat)*
1. Vaccines · Vacinas
2. Deworming / flea treatments · Desparasitação / pulgas
3. Medications · Medicação
4. Weight · Peso
5. Food / diet / water intake · Alimentação / dieta / ingestão de água
6. Litter box / urinary health · Caixa de areia / saúde urinária
7. Dental care · Saúde oral
8. Grooming / hairballs · Escovagem / bolas de pelo
9. Vet visits / test results · Consultas / resultados de análises
10. Microchip / passport / registration · Microchip / passaporte / registo (SIAC)
11. Expenses · Despesas

### C3 · Small mammals *(show if A2 contains Rabbit…)*
1. Vaccines (e.g. rabbit RHD / myxomatosis) · Vacinas (ex.: DHV / mixomatose em coelhos)
2. Weight · Peso
3. Diet (hay / pellets / fresh food) · Alimentação (feno / ração / frescos)
4. Teeth · Dentes
5. Nail trims · Corte de unhas
6. Cage / enclosure cleaning · Limpeza da jaula / recinto
7. Appetite / droppings · Apetite / fezes
8. Medications · Medicação
9. Vet visits · Consultas veterinárias
10. Breeding / litters · Reprodução / ninhadas

### C4 · Birds *(show if A2 contains Bird)*
1. Weight (grams) · Peso (gramas)
2. Diet / seeds / pellets / fresh food · Alimentação / sementes / papa / frescos
3. Molting · Muda de penas
4. Beak / nail / wing trims · Corte de bico / unhas / asas
5. Cage cleaning · Limpeza da gaiola
6. Behaviour / feather plucking · Comportamento / arrancar penas
7. Eggs / breeding · Ovos / reprodução
8. Ring / band ID / CITES documents · Anilha / documentos CITES
9. Medications / supplements · Medicação / suplementos
10. Vet visits · Consultas veterinárias

### C5 · Reptiles and amphibians *(show if A2 contains Reptile)*
1. Feeding (prey type / size / date) · Alimentação (tipo / tamanho da presa / data)
2. Refused meals · Refeições recusadas
3. Sheds (complete / incomplete) · Mudas de pele (completas / incompletas)
4. Weight / length · Peso / comprimento
5. Temperature · Temperatura
6. Humidity · Humidade
7. UVB bulb replacement · Substituição da lâmpada UVB
8. Supplements (calcium / D3 / vitamins) · Suplementos (cálcio / D3 / vitaminas)
9. Enclosure cleaning · Limpeza do terrário
10. Brumation / hibernation · Brumação / hibernação
11. Breeding / eggs / incubation · Reprodução / ovos / incubação
12. Vet visits · Consultas veterinárias
13. CITES / legal documents · CITES / documentos legais

### C5b · Reptile logging method *(show if A2 contains Reptile)*
- **Block:** Checkboxes · Optional
- **EN:** How do you log this today?
- **PT:** Como regista isto hoje?
- **Options:** Memory / De memória · Paper log on the enclosure / Registo em papel junto ao terrário · Spreadsheet / Folha de cálculo · Notes app / App de notas · Reptile-specific app / App específica para répteis · Other / Outro

### C6 · Fish / aquarium / pond *(show if A2 contains Fish)*
1. Water parameters (pH / ammonia / nitrite / nitrate) · Parâmetros da água (pH / amónia / nitritos / nitratos)
2. Water temperature · Temperatura da água
3. Water changes · Mudanças de água
4. Filter / equipment maintenance · Manutenção do filtro / equipamento
5. Feeding · Alimentação
6. Livestock list (what's in the tank) · Lista de animais no aquário
7. Illness / deaths · Doenças / mortes
8. Plants / corals · Plantas / corais
9. Medications / treatments · Medicação / tratamentos
10. Expenses · Despesas

### C7 · Horses *(show if A2 contains Horse)*
1. Farrier / hooves · Ferrador / cascos
2. Dental · Dentes
3. Deworming · Desparasitação
4. Vaccines · Vacinas
5. Equine passport / documents · Passaporte equino / documentos
6. Feed / supplements · Alimentação / suplementos
7. Exercise / training / competitions · Exercício / treino / provas
8. Injuries / lameness · Lesões / claudicação
9. Stable / livery costs · Custos de estábulo / pensão
10. Vet visits · Consultas veterinárias

### C8 · Farm / invertebrates / other *(show if A2 contains Farm OR Invertebrates OR Other)*
- **Block:** Long answer · Optional
- **EN:** Tell us which animals and what you need to keep track of for them.
- **PT:** Diga-nos que animais tem e o que precisa de acompanhar/registar para eles.

### C9 · Hardest animal (multi-species only)
- **Block:** Multiple choice · Optional
- **Logic:** Show if A3 ≠ "1"
- **EN:** Which of your animals takes the most effort to keep track of?
- **PT:** Qual dos seus animais dá mais trabalho a acompanhar?
- **Options:** same 10 options as A2 (EN/PT)

**— Page break —**

---

## 6. Section D — What would help

### D1 · Feature importance
- **Block:** **Matrix** (rows × columns, single choice per row) · **Required**
- *If Matrix isn't available on your plan, use one **Linear scale 1–4** block per row and label 1 = Not important, 4 = Must have.*
- **EN:** How important would each of these be for you?
- **PT:** Que importância teria cada uma destas funcionalidades para si?

**Columns:** Not important / Nada importante · Nice to have / Seria simpático · Important / Importante · Must have / Essencial

| ID | EN row | PT row |
|---|---|---|
| D1.1 | Reminders for vaccines / medications / treatments | Lembretes de vacinas / medicação / tratamentos |
| D1.2 | All health records and documents in one place | Todo o historial de saúde e documentos num só sítio |
| D1.3 | One-tap logging (fed / walked / meds given) | Registo com um toque (alimentado / passeado / medicação dada) |
| D1.4 | Sharing with family / sitters with live "done" status | Partilha com família / pet sitters com estado "feito" em tempo real |
| D1.5 | Vet-ready health summary (PDF or link) | Resumo de saúde pronto para o veterinário (PDF ou link) |
| D1.6 | Scan vaccine cards / invoices automatically | Digitalizar boletins de vacinas / faturas automaticamente |
| D1.7 | Care tracking specific to my species | Acompanhamento específico para a minha espécie |
| D1.8 | Weight and health trends / alerts | Evolução do peso e da saúde / alertas |
| D1.9 | Expense tracking | Controlo de despesas |
| D1.10 | Lost-pet QR tag / emergency profile | Medalha QR / perfil de emergência para animal perdido |
| D1.11 | Travel / passport / vaccine-validity checklist | Checklist de viagem / passaporte / validade de vacinas |
| D1.12 | Care guides for my species | Guias de cuidados para a minha espécie |
| D1.13 | Social feed / community with other owners | Feed social / comunidade com outros tutores |

> D1.13 is deliberately included as a **control**. It tests whether social features really matter to users, which the roadmap says to postpone.

### D2 · One thing
- **Block:** Short answer · Optional
- **EN:** If a pet app could do just ONE thing perfectly for you, what should it be?
- **PT:** Se uma app para animais pudesse fazer só UMA coisa na perfeição, o que deveria ser?

### D3 · Sharing
- **Block:** Checkboxes · **Required**
- **EN:** Who would you want to share your animals' info with? *Select all that apply.*
- **PT:** Com quem gostaria de partilhar a informação dos seus animais? *Selecione todas as opções aplicáveis.*

| # | EN | PT |
|---|---|---|
| 1 | Partner / family | Companheiro(a) / família |
| 2 | Pet sitter / walker | Pet sitter / passeador |
| 3 | Vet | Veterinário |
| 4 | Breeder / shelter I got the animal from | Criador / associação de onde veio o animal |
| 5 | Friends / neighbours | Amigos / vizinhos |
| 6 | Nobody | Ninguém |

### D4 · Past app use
- **Block:** Multiple choice · **Required**
- **EN:** Have you ever used a pet care app?
- **PT:** Já alguma vez usou uma app de cuidados para animais?
- **Options:** Yes, and I still use it / Sim, e ainda uso · Yes, but I stopped / Sim, mas deixei de usar · No, never / Não, nunca

### D5 · Why stopped
- **Block:** Checkboxes · Optional
- **Logic:** Show if D4 = "Yes, but I stopped"
- **EN:** Why did you stop? *Select all that apply.*
- **PT:** Porque deixou de usar? *Selecione todas as opções aplicáveis.*

| # | EN | PT |
|---|---|---|
| 1 | Too much typing / too much effort | Dava muito trabalho a preencher |
| 2 | Didn't support my type of animal | Não suportava o meu tipo de animal |
| 3 | Reminders didn't work well | Os lembretes não funcionavam bem |
| 4 | Important features were paid | Funcionalidades importantes eram pagas |
| 5 | Too complicated / cluttered | Demasiado complicada / confusa |
| 6 | Couldn't share with others | Não dava para partilhar com outras pessoas |
| 7 | I just forgot about it | Simplesmente esqueci-me dela |
| 8 | Lost my data / changed phone | Perdi os dados / mudei de telemóvel |
| 9 | Not available in my language | Não estava disponível na minha língua |
| 10 | Other | Outro |

**— Page break —**

---

## 7. Section E — Paying for it

*Note: stated prices are only indicative. Treat these answers as a range, not a forecast. The real test is the paywall A/B test in Phase 2.*

### E1 · Current paid pet services
- **Block:** Checkboxes · **Required**
- **EN:** Do you currently pay for any of these? *Select all that apply.*
- **PT:** Atualmente paga algum destes serviços? *Selecione todos os que se aplicam.*

| # | EN | PT |
|---|---|---|
| 1 | Pet insurance | Seguro para animais |
| 2 | Vet wellness / health plan | Plano de saúde veterinário |
| 3 | GPS tracker subscription | Subscrição de localizador GPS |
| 4 | Pet app subscription | Subscrição de app para animais |
| 5 | Food delivery subscription | Subscrição de entrega de ração |
| 6 | None of these | Nenhum destes |

### E2 · Payment preference
- **Block:** Multiple choice · **Required**
- **EN:** If an app solved your biggest pet-care headache, how would you prefer to pay?
- **PT:** Se uma app resolvesse a sua maior dor de cabeça com os animais, como preferiria pagar?

| # | EN | PT |
|---|---|---|
| 1 | Free version only, I wouldn't pay | Só a versão gratuita, não pagaria |
| 2 | One-time purchase | Pagamento único |
| 3 | Monthly subscription | Subscrição mensal |
| 4 | Yearly subscription (cheaper per month) | Subscrição anual (mais barata por mês) |
| 5 | Not sure | Não sei |

### E3 · Fair price
- **Block:** Multiple choice · Optional
- **Logic:** Hide if E2 = option 1
- **EN:** What monthly price would feel fair for a premium version (unlimited animals / sharing with sitters / automatic document scanning)?
- **PT:** Que preço mensal lhe pareceria justo para uma versão premium (animais ilimitados / partilha com pet sitters / digitalização automática de documentos)?
- **Options:** Up to €1.99 / Até 1,99€ · €2–3.99 / 2–3,99€ · €4–5.99 / 4–5,99€ · €6–9.99 / 6–9,99€ · €10 or more / 10€ ou mais

> PT labels use a decimal comma ("1,99€"). That's fine here because this is a single-choice question.

**— Page break —**

---

## 8. Section F — Stay in touch

### F1 · Follow-up options
- **Block:** Checkboxes · Optional
- **EN:** Would you like to… *Select all that apply.*
- **PT:** Gostaria de… *Selecione todas as opções aplicáveis.*

| # | EN | PT |
|---|---|---|
| 1 | Join the beta / early access waitlist | Entrar na lista de espera para a versão beta |
| 2 | Do a 20-minute video call to share your experience | Participar numa videochamada de 20 minutos para partilhar a sua experiência |
| 3 | Receive the survey results | Receber os resultados deste inquérito |

### F2 · Email
- **Block:** Email · **Required when shown**
- **Logic:** Show if F1 contains any option
- **EN:** Your email
- **PT:** O seu email

### F3 · Call language
- **Block:** Multiple choice · Optional
- **Logic:** Show if F1 contains option 2
- **EN:** Which language would you prefer for the call?
- **PT:** Em que língua prefere a chamada?
- **Options:** Portuguese / Português · English / Inglês · Spanish / Espanhol · Either / Tanto faz

### F4 · Consent (GDPR)
- **Block:** Checkbox (single) · **Required when shown**
- **Logic:** Show if F1 contains any option
- **EN:** I agree that PetPaw may store my email to contact me about the options I selected above. I can ask for it to be deleted at any time by replying to any email.
- **PT:** Aceito que a PetPaw guarde o meu email para me contactar sobre as opções que selecionei acima. Posso pedir a sua eliminação a qualquer momento respondendo a qualquer email.

### F5 · Anything else
- **Block:** Long answer · Optional
- **EN:** Anything else you'd like to tell us?
- **PT:** Há mais alguma coisa que nos queira dizer?

### Thank-you page
- **EN:** 🎉 **Thank you!** You're helping build something for every animal out there. If you know someone with an unusual pet (or just a lot of pets!), please share this survey: `[link]`
- **PT:** 🎉 **Obrigado!** Está a ajudar a criar algo para todos os animais. Se conhece alguém com um animal diferente (ou com muitos animais!), partilhe este inquérito: `[link]`

---

## 9. Codebook and analysis plan

### Codebook (rename your analysis-sheet columns to these IDs)

| ID | Question | Type | Notes |
|---|---|---|---|
| `lang` | hidden | text | en / pt |
| `src` | hidden | text | channel |
| `A1_status` | Pet status | single | |
| `A2_species` | Species | multi | Split into flags (below) |
| `A3_count` | Number of animals | single | |
| `A4_carers` | Who cares | multi | |
| `A5_role` | Role | multi | |
| `A6_country` | Country | single | |
| `A7_age` | Age | single | |
| `B1_tools` | Current tracking tools | multi | |
| `B2_app_name` | App used | text | Normalise names manually |
| `B3_pains` | Pain events | multi | **Key metric** |
| `B4_story` | Story | long text | Theme-code (below) |
| `B5_vet_freq` | Vet frequency | single | |
| `B6_spend` | Monthly spend | single | |
| `C1`–`C8` | Species tracking | multi / text | One column per species |
| `C5b_reptile_method` | Reptile logging method | multi | |
| `C9_hardest` | Hardest animal | single | |
| `D1.1`–`D1.13` | Feature importance | 1–4 | Map: Not important = 1 … Must have = 4 |
| `D2_one_thing` | One thing | text | Theme-code |
| `D3_share_with` | Share with | multi | |
| `D4_app_use` | Past app use | single | |
| `D5_why_stopped` | Why stopped | multi | **Churn reasons** |
| `E1_paid_now` | Paid services | multi | |
| `E2_pay_pref` | Payment preference | single | |
| `E3_fair_price` | Fair price | single | |
| `F1_followup` | Follow-up options | multi | |
| `F2_email` | Email | text | **Personal data**: keep it in a separate, restricted tab |
| `F3_call_lang` | Call language | single | |
| `F5_other` | Anything else | long text | |

### Setting up the sheet
1. **Two raw tabs** (`raw_EN`, `raw_PT`) fed by Tally. **Never edit these.**
2. An **`all` tab** that stacks both and translates PT answers into EN labels:
   `=VSTACK(raw_EN!A2:AZ, raw_PT!A2:AZ)`, then a `lookup` tab (PT label → EN label) used with `SUBSTITUTE`/`XLOOKUP` on the multi-select columns.
   *Simpler alternative:* since the option **order** matches, you can export both as CSV and ask Claude to merge and normalise them.
3. **Flag columns** for multi-selects, e.g. a column `has_reptile`:
   `=IF(ISNUMBER(SEARCH("Reptile", C2)), 1, 0)`
4. **Segments** (computed columns):
   - `is_exotic` = has a bird, reptile, fish, small mammal, horse, farm or invertebrate
   - `is_multispecies` = number of species flags ≥ 2
   - `is_shared_care` = A4 doesn't contain "Only me"
   - `is_app_churned` = D4 = "Yes, but I stopped"
   - `pain_count` = number of B3 items selected (excluding "None")

### Key questions to answer (and how)

| Question | How |
|---|---|
| How big is the multi-species / exotic segment? | % `is_multispecies`, % `is_exotic` (and by `src`, since Reddit will skew exotic) |
| Which pains are most common, and for whom? | % of each B3 option, overall vs `is_exotic` vs `is_shared_care` |
| What should the MVP prioritise? | Mean D1 score per feature, plus % "Must have". Rank them, then compare segments. |
| Does sharing matter? | D1.4 score among `is_shared_care` vs solo owners. D3 distribution. |
| Why do pet apps lose users? | D5 counts (the top 3 are your design constraints) |
| Is the social feed really low priority? | D1.13 vs the rest |
| Is paying plausible? | E2 × E1 (people who already pay for pet services are more likely to pay), E3 distribution |
| Which species templates come first? | A2 frequency × C9 "hardest" × C-block item frequency |

### Coding open text (B4, D2, F5)
Start with these theme codes and add new ones as they appear:
`FORGOT_TREATMENT` · `DOUBLE_DOSE` · `LOST_DOCS` · `VET_HISTORY` · `SITTER_INFO` · `TRAVEL_DOCS` · `LATE_HEALTH_SIGNAL` · `SPECIES_UNSUPPORTED` · `TOO_MUCH_TYPING` · `COST` · `OTHER`.
Tag each answer with 1–2 codes in a `B4_codes` column, then count them. You can paste the column into Claude to draft the codes and then review them yourself.

### When you have enough data
- **Minimum to read percentages:** about 100 complete responses. **Comfortable:** 200+.
- **For segment comparisons** (e.g. exotic vs dog/cat), aim for ≥30 per segment before drawing conclusions.
- Don't treat it as representative of all owners, because the channel decides who answers. Report results **by `src`**.

---

## 10. Launch checklist

- [ ] Build the EN form and test **every logic branch** (No pets, under 18, each species, stopped app use, no follow-up).
- [ ] Duplicate it to PT and check that the option order matches EN exactly.
- [ ] Connect both forms to Google Sheets and check that hidden fields arrive.
- [ ] **Pilot with 5 people** (2 dog/cat, 2 exotic, 1 non-tech). Time them, and stop and fix anything that takes >8 minutes or confuses anyone.
- [ ] Add a short privacy note link (a simple Notion or Tally page saying what you collect, why, and how to delete it).
- [ ] Prepare tracked links per channel (`?src=`).
- [ ] Post (see templates below). Reply to comments, because it boosts reach on Reddit and Facebook.
- [ ] Check the Sheet at 25, 50 and 100 responses. If a question gets >30% "Other", fix its options.
- [ ] Email interview volunteers within 48 hours while they're still motivated.

### Distribution templates

**EN (Reddit / Discord):**
> Hi all! I'm building a free app to track care for *any* animal (not just dogs and cats): feeding, sheds, weights, vet records, reminders, sharing with sitters. Before building anything, I'd love to hear how you track things today. It's a 6-min anonymous survey: [link]. I'll share the results back here. Mods, please remove if not allowed 🙏

**PT (Facebook groups / WhatsApp):**
> Olá a todos! Estou a criar uma app gratuita para ajudar a cuidar de qualquer animal (não só cães e gatos): vacinas, desparasitações, alimentação, peso, consultas, lembretes e partilha com quem cuida deles. Antes de começar, queria perceber como cada um faz hoje. São 6 minutos e é anónimo: [link]. Depois partilho os resultados aqui. Obrigado! 🐾

**Suggested channels:** r/reptiles, r/leopardgeckos, r/ballpython, r/parrots, r/budgies, r/Rabbits, r/Aquariums, r/Equestrian, r/dogs, r/cats · Portuguese Facebook groups for dogs, cats, exotic animals and adoption · local vet clinics and shelters (printed QR poster) · reptile and bird expos · friends and family (tag `src=friends` and analyse separately).

> Check each community's rules before posting. Many subreddits ban surveys or require mod approval, so message the mods first.
