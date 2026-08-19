# Ilgʻor harness dizaynlari tahlili

Bu rukn kurs maʼruzalarida bayon qilingan harness nazariyasini hozirgi eng ilgʻor real mahsulotlar bilan birma-bir taqqoslaydi. Har bir mahsulotda bizni faqat bir narsa qiziqtiradi: **uning harnessʼi qanday loyihalangan** — yaʼni model atrofidagi muhandislik infratuzilmasi: yoʻriqnomalar, vositalar, muhit, holat va qayta aloqa kabi beshta quyi tizim, shuningdek kontekst uzluksizligi, inisializatsiya, tekshiruv, kuzatuvchanlik, topshirish va sikl kabi asosiy mexanizmlar.

Biz ataylab modelning fikrlash qobiliyati kuchlimi yoki yoʻqmi, muayyan benchmark natijasi yuqorimi yoki yoʻqmi degan mavzularni muhokama qilmaymiz. Shuningdek, “bu agent nima qila oladi” degan umumiy tanishtiruvni ham bermaymiz. Bular model va mahsulot qatlamlariga oid masalalar. Bu yerda faqat harness — model ogʻirliklaridan tashqaridagi barcha narsalar — tahlil qilinadi.

## Nega tahlil qilishga arziydi

Birinchi maʼruzada aytilganidek, modelning kuchli boʻlishi bajarilishning ishonchli boʻlishini anglatmaydi. Bir xil model turli harness ichida ishlatilsa, natijalar bir daraja miqyosida farq qilishi mumkin. Ammo maʼruzalar “qanday qilish kerak”ligini tushuntirsa, bu mahsulotlar “yetakchi jamoalar amalda qanday qilmoqda” degan savolga javob beradi.

Har bir mahsulot — mustaqil dizayn qarorlari toʻplami. Ularni yonma-yon taqqoslasangiz, ayni asosiy mexanizmlarni turli jamoalar mutlaqo boshqacha usullarda amalga oshirganini koʻrasiz:

- **Pi** harnessʼni minimal yadro + dasturlashtiriladigan kengaytmalar shaklida quradi va “minimal system prompt + talab boʻyicha yuklash” orqali kontekst muhandisligini amalga oshiradi.
- **Claude Code** harnessʼni toʻliq runtime muhitiga aylantiradi: qatlamli xotira, besh bosqichli siqish, ruxsatlar, hookʼlar va subagentlar.
- **Codex** harness falsafasini eng oxirgi nuqtasigacha olib boradi: repo yagona haqiqat manbai, AGENTS.md esa faqat mundarija sahifasi; muhit worktree yordamida izolyatsiya qilinadi.
- **DeepSeek Harness** harnessʼning oʻzini modeldan mustaqil runtime sifatida belgilaydi: Everything is a Plugin.

## Maqolalar roʻyxati

- [Pi harness dizayni tahlili](./pi/): minimal yadro + dasturlashtiriladigan kengaytmalar; kontekst muhandisligi system prompt tashqarisiga olib chiqilgan.
- [Claude Code harness dizayni tahlili](./claude-code/): qatlamli xotira, besh bosqichli siqish, ruxsatlar va hookʼlar — toʻliq agent runtime muhiti.
- [Codex harness dizayni tahlili](./codex/): repo yagona haqiqat manbai, AGENTS.md mundarija sahifasi, muhit izolyatsiyasi va qayta aloqa sikli.
- [DeepSeek Harness dizayni tahlili](./deepseek/): Everything is a Plugin; hatto agent siklining oʻzi ham almashtiriladigan plaginga aylantirilgan.

## Qanday oʻqish kerak

Avval beshta quyi tizim modelini tushunib olish uchun maʼruzalarning dastlabki qismlarini (ayniqsa [2-maʼruza: Harness aslida nima?](../lectures/lecture-02-what-a-harness-actually-is/)) oʻqishni, soʻng real mahsulotlar bu mexanizmlarni qanday amalga oshirganini koʻrish uchun bu yerga qaytishni tavsiya qilamiz.

Har bir maqola oxirida “Kurs modeliga moslashtirish” va “Oʻrganishga arziydigan dizaynlar” boʻlimlari bor. Ular mahsulot dizaynini kurs tushunchalariga tezda aylantirish va bevosita oʻz loyihangizga tatbiq etishga yordam beradi.
