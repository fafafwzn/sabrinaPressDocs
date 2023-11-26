# Sabrina BotPress
Sabrina BotPress merupakan customer-facing chatbot milik BRI yang memadukan sistem chatbot rule-based dengan kapabilitas LLM serta Retrieval-Augmented Generation (RAG). Chatbot ini memiliki beragam fitur, yaitu:
* [Main Flow](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#main-flow)
* [Product Information](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-product-information)
* [Complaint Handling](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-complaint-handling)
* [General Question](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-general-question)
* [Fund Transfer](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-fund-transfer)
* [Balance Checking](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-balance-checking)
* [Score Checking](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-score-checking)
* [Investment Advisor](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-investment-advisor)
* [Location Information](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-location-information)

Pada Sabrina BotPress terdapat beberapa komponen lain seperti [Dokumen, Tabel, dan API](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#documents-tables--apis).

## Platform Overview
### Nodes & Cards
Node: Sebuah elemen yang apabila ter-trigger akan mengeksekusi card yang berada di dalamnya secara berurutan dari atas hingga ke bawah. Node memiliki 1 input dan 1 atau lebih output yang akan ter-trigger apabila kondisi tertentu terpenuhi. Untuk memastikan node memiliki keluaran, letakkan card "Expression" dengan condition "true" di bagian paling akhir dari suatu node. Terdapat dua capability yang dapat ditoggle pada masing-masing node di Botpress, yaitu "Enable Knowledge Answering" yaitu kemampuan untuk langsung memberikan jawaban dari dokumen jika card Capture Information pada node tersebut menerima masukan dari user, dan "Enable Personality Rewriting" yang dapat memproses semua teks yang dikeluarkan oleh chatbot. Pemrosesan terhadap teks keluaran chatbot tersebut tergantung pada agent-agent yang diaktifkan. Penjelasan terkait agent akan dibahas secara detail pada sub-bagian berikutnya.\
\
Card: Elemen yang lebih kecil daripada node dan berperilaku sesuai dengan jenisnya.
* Intent: Mentrigger output dari node apabila chatbot menerima pesan dari user yang memiliki suatu intent tertentu yang telah didefinisikan pada bagian pengaturan dari card tersebut
* Expression: Mentrigger output dari node apabila suatu kondisi yang didefinisikan dalam codingan pada bagian Condition di pengaturan card bernilai "true". Best practice dari card ini adalah untuk mengkombinasikan dengan variable, kemudian menuliskan kondisi yang diinginkan pada kolom "Label", kemudian Generative AI akan secara otomatis menuliskan codingan yang sesuai dengan kondisi yang kita inginkan di bagian Condition
* AI Task: "Task Instructions" diisikan prompt template berisi perintah kita. "AI Task Input" diisikan masukan, best practicenya diisi dengan variabel {{event.preview}} jika inputnya adalah pesan terbaru dari user, atau dapat juga diset ke variabel tertentu apabila masukannya adalah keluaran dari proses yang lain. "Store result in variables" diisikan variabel yang ingin kita influence dengan AI Task ini. Bisa kita isikan variabel yang menangkap keluaran dari Generative AI, atau bisa juga kita berikan variable boolean yang bisa dialter oleh Generative AI sesuai dengan hasil klasifikasinya (jika kita berikan Task Instructions untuk mengklasifikasikan). "Example Input" berisi example few-shot, cara yang baik untuk cheating jawaban
* Execute Code: Kita dapat menuliskan script dalam Javascript ke dalam execute code ini. Best practicenya adalah untuk memanipulasi nilai atau tipe data dari variabel tertentu, membuat logic if-else atau looping, dan melakukan hit API tertentu jika suatu kondisi terpenuhi
* Raw Input (Capture Information Cards): Berguna untuk menangkap masukan dari user. Pada bagian pengaturan, di bagian "Type of value to extract" kita dapat mensetting tipe data yang akan diterima dari pesan user, di bagian "Question to ask to the user" kita menuliskan pertanyaan yang akan ditanyakan kepada user saat meminta mereka untuk mengetikkan sesuatu, kemudian di bagian "Store result in" dapat kita isikan variabel tempat kita menyetorkan masukan dari user (variabel ini akan memiliki tipe data yang sesuai dengan pilihan kita pada bagian "Type of value to extract"). Raw input juga dapat diatur lebih lanjut dengan mengeklik tombol "Advanced Configuration". Pengaturan lanjutan disediakan yaitu "Validation" untuk melakukan validasi terhadap input user yang mana validasi tersebut dilakukan berdasarkan pencocokan dengan kondisi yang diatur melalui script validator. Kemudian "Number of retries" untuk mensetting jumlah maksimal kesalahan pengisian oleh user. Kemudian "Extract from history" untuk mengikutsertakan pesan-pesan sebelumnya ke dalam variabel (history yang terbawa maksimal hanya 5 pesan sebelumnya). Kemudian "Choice" untuk menyediakan pilihan ganda untuk dipilih oleh user, jika nantinya user memilih salah satu dari pilihan yang disediakan, maka user inputnya akan berupa pilihan user tersebut
* Triggers: Pemicu dimulainya percakapan dari node tertentu. Dapat dipilih menggunakan pemicu "Conversation Started" atau "Custom Trigger (advanced)" yang pemicunya adalah adanya payload yang masuk (dapat diakses di variabel ({{event.payload}})

### Agents
1. Summary Agent
Agent ini berfungsi untuk meringkas masukan dari user. Hasil ringkasan dapat diatur jumlah tokennya serta jumlah chat yang disummary. Hasil ringkasan tersebut dapat diakses melalui variabel conversation.SummaryAgent.summary. Sedangkan transkrip lengkap dari riwayat percakapan user dengan chatbot dapat diakses melalui variabel conversation.SummaryAgent.transcript, namun perlu dicatat bahwa terkadang riwayat percakapan yang ada pada variabel tersebut tidak lengkap
2. Personality Agent
Agent ini berfungsi untuk membuat chatbot memiliki kepribadian khusus yang dapat diatur melalui Personality Description
3. Knowledge Agent
Agent ini berfungsi untuk mengatur cara chatbot menjawab dari Knowledge Base. Kita dapat mengatur prompt yang digunakan untuk menginstruksikan cara meretrieve informasi dari dokumen yang kita upload ke Knowledge Base. *Catatan penting*: Ketika pertama kali membuat chatbot baru di Botpress, konfigurasi "Answer on start node" secara otomatis diaktifkan. Sehingga jika ada dokumen yang diupload ke Knowledge Base, maka seluruh masukan dari user akan dijawab menggunakan Knowledge Base tersebut
4. Translator Agent
Agent ini berfungsi untuk menerjemahkan jawaban dari chatbot ke bahasa masukan user

### Variables
| *Variabel* | *Penjelasan* |
| :---------: | :--------- |
| event.preview | Pesan user terakhir |
| event.payload | Payload yang diterima |
| user.user_acctno | Nomor rekening user (dummy), digunakan sebagai identifier utama di Sabrina BotPress |
| user.user_input | Masukan dari user yang akan diset sebagai masukan AI Task pada controller node di main flow |
| other sub-flow variables | Variabel-variabel lainnya yang akan dirinci pada penjelasan masing-masing sub-flow |

## Main Flow
### Workflow
1. Pada setiap awal session, user akan disambut dengan sapaan pembuka dari Sabrina. Di tahap ini, user bisa memilih topik yang disediakan atau mengetik langsung pertanyaan mereka. Pada BotPress studio, masukan user akan masuk ke node main_input. Jika user memilih topik "Informasi Produk", "Komplain", atau "Pertanyaan Umum BRI", maka mereka akan diarahkan ke node main_prompting. Jika user memilih topik selain 3 topik tersebut, maka akan langsung diarahkan ke sub-flow. Jika user mengetik pertanyaannya langsung, maka user akan masuk ke node main_controller.\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/153d654d-4f8c-4770-ac4e-c30822788832)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/38fd56ae-f655-4da6-b931-589e298af39c)

2. Jika user diarahkan ke node main_prompting, maka user akan diberikan pertanyaan lanjutan. Card AI Task pada node ini berfungsi untuk membuat pertanyaan sesuai dengan pilihan user ("Informasi Produk", "Komplain", atau "Pertanyaan Umum BRI"), kemudian pertanyaan tersebut disimpan pada variabel workflow.botQuestion dan ditampilkan pada card Raw Input di bawahnya. Kemudian input dari user diteruskan ke node main_controller\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/8071e806-645d-4c55-b4aa-45acdbb9fb32)

3. Node main_controller berperan sebagai controller, yaitu pengontrol percakapan yang bertugas untuk mengklasifikasikan pertanyaan dari user ke dalam masing-masing fitur chatbot\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/54fe3aef-f5ef-4be1-a12a-5f2d45a7ac53)

Node ini berisi 14 card yang fungsinya adalah sebagai berikut:\
Card pertama adalah "General Intent", card ini berfungsi untuk menyeleksi pesan user yang bersifat generic untuk diteruskan ke node main_prompting sehingga user diberikan pertanyaan lanjutan. Card berikutnya adalah AI Task yang berfungsi untuk mengklasifikasikan pesan user ke dalam sub-flow yang sesuai dengan intensinya. Cara kerjanya adalah dengan memberikan AI Task beberapa variabel boolean yang masing-masing secara default bernilai "false", kemudian memberikan instruksi kepada AI Task untuk mengubah salah satunya menjadi bernilai "true" apabila kondisi yang kita berikan pada bagian Task Instructions terpenuhi.
* Apabila variabel workflow.productInformation bernilai "true", maka pesan user akan diteruskan ke sub-flow [Product Information](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-product-information)
* Apabila variabel workflow.userComplaint bernilai "true", maka pesan user akan diteruskan ke sub-flow [Complaint Handling](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-complaint-handling)
* Apabila variabel workflow.generalQuestion bernilai "true", maka pesan user akan diteruskan ke sub-flow [General Question](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-general-question)
* Apabila variabel workflow.fundTransfer bernilai "true", maka pesan user akan diteruskan ke sub-flow [Fund Transfer](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-fund-transfer)
* Apabila variabel workflow.balanceCheck bernilai "true", maka pesan user akan diteruskan ke sub-flow [Balance Checking](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-balance-checking)
* Apabila variabel workflow.creditScoring bernilai "true", maka pesan user akan diteruskan ke sub-flow [Score Checking](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-score-checking)
* Apabila variabel workflow.investmentAdvisor bernilai "true", maka pesan user akan diteruskan ke sub-flow [Investment Advisor](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-investment-advisor)
* Apabila variabel workflow.locInfo bernilai "true", maka pesan user akan diteruskan ke sub-flow [Location Information](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-location-information)
* Apabila variabel workflow.needPrompting bernilai "true", maka pesan user akan diteruskan ke node main_reinput. Variabel ini akan bernilai true jika user baru saja keluar dari sub-flow dan variabel user.user_input bernilai "need_prompting"
> Catatan: Kondisi user keluar dari sub-flow ada dua jenis, yang pertama yaitu keluar karena sub-flow tersebut sudah tereksekusi hingga End nodenya, dan yang kedua yaitu keluar karena user tiba-tiba memberikan tanggapan di luar konteks sub-flow tersebut sebelum menyelesaikannya hingga ke End node. Kondisi kedua merupakan kondisi yang akan membuat variabel user.user_input bernilai "need_prompting" sehingga memicu card AI Task pada node main_controller untuk mengubah variabel workflow.needPrompting menjadi bernilai "true"
* Apabila variabel workflow.endNode bernilai "true", maka pesan user akan diteruskan ke node main_conclude dan percakapan akan diakhiri
\
\
Berikut ini adalah variabel masukan dan keluaran dari card AI Task pada node main_controller:
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/99b84039-a12b-4377-b9be-fcb96d5f59d3)

Card nomor 3-12 pada node main_controller merupakan card expression yang masing-masing akan meneruskan output dari node main_controller ke sub-flow atau ke node lainnya sesuai dengan uraian sebelumnya. Kemudian card nomor 13 adalah "Product Name", card ini akan mengarahkan pesan user yang tidak terklasifikasi ke card-card expression sebelumnya namun mengandung nama produk BRI dan mengarahkannya ke sub-flow Product Information. Kemudian yang terakhir adalah expression card dengan Label "always" untuk mengarahkan masukan user yang tidak terklasifikasi ke card manapun ke node fallback\
\
4. Pesan user yang tidak masuk ke sub-flow manapun seperti misalnya small talk, sapaan, salam, dan pertanyaan-pertanyaan di luar konteks akan diteruskan oleh node main_controller menuju ke node main_fallback. Di node ini terdapat AI Task yang diinstruksikan untuk membuat pesan balasan terhadap masukan user, dan diikuti dengan follow-up question yang akan menanyakan kembali kebutuhan user seperti pada sapaan pembuka di awal session\
\
5. Apabila pesan user diteruskan menuju node main_reinput, berarti user telah menyelesaikan sub-flow dari start node hingga ke end nodenya, kemudian dikembalikan ke main flow menuju ke node main_controller, dan diteruskan ke node main_reinput. Pada node ini, user akan diberikan kesempatan untuk mengajukan pertanyaan lainnya seperti pada sapaan pembuka di awal session\
\
6. Node main_conclude berisi penutup percakapan\
\
\
Cara kerja main flow secara umum adalah sebagai berikut:\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/20a7b3ad-c754-40e5-a18f-d38170d6c081)


## Sub-Flow Product Information
### User intent example
### Workflow
### Variables

## Sub-Flow Complaint Handling
### User intent example
* saya gagal login brimo
* brimo bermasalah
* tidak dapat verifikasi wajah di brimo
* pengajuan ceria gagal
* lupa username dan password brimo
* brimo saya terblokir
* sy mau mengajukan komplain
* brimo saya mengalami kendala
* brimo bermasalah
* lupa pin brimo

### Workflow
1. Saat user mengajukan suatu komplain, maka controller di main flow akan mengarahkan ke sub-flow complaint handling
2. Di sub-flow complaint handling, mula-mula flow dari user akan disummarize untuk dikonfirmasi oleh user. Proses summarize tersebut terjadi pada card AI Task pertama pada node complaint_summarize. User dapat melakukan konfirmasi dengan cara menekan tombol benar/salah pada chat interface, atau dengan mengetik. Jika masukan dari user berupa ketikan, maka card AI Task kedua pada node complaint_summarize akan mengklasifikasikan ketikan user tersebut (termasuk afirmatif atau non-afirmatif)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/dcebf493-2053-48ef-8267-5135cfa32acd)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/60ed620a-f4cb-4914-bf25-9a8c6a093c46)
3. Jika user tidak mengkonfirmasi hasil summarize terhadap komplain mereka, maka mereka akan diarahkan ke node complaint_fallback. Pada node ini, user diminta untuk memparafrase masukan mereka. Kemudian, output dari node ini akan diarahkan kembali ke main flow, tepatnya menuju ke node controller untuk nantinya diarahkan kembali ke sub-flow complaint handling jika masukan user pada card input di node complaint_fallback bersifat komplain\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/029ebeba-3d53-4cba-bb65-2473eaa90138)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/3f12cc2f-b388-41cb-8120-cf8dc4299db2)
4. Jika user mengkonfirmasi hasil summarize terhadap komplain mereka, maka komplain mereka akan dijawab oleh card knowledge-query pada node complaint_KB yang akan mencari jawaban atas komplain user pada Complaint Handling Knowledge Document. Pada node ini, jika komplain dari user tidak dapat dijawab menggunakan informasi yang disediakan pada Complaint Handling Knowledge Document, maka akan diarahkan ke node complaint_ooc, lalu diteruskan ke main flow\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/802beef1-2b91-4204-a2d1-41a71ffc9cc1)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/420c8670-d069-407e-9b3d-e027efe0d9ea)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/7ff1d038-41b5-42ce-9490-0b21d733dab9)
\
Cara kerja sub-flow complaint handling secara umum adalah sebagai berikut:\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/cf4c6cb1-1c5c-48bb-bdee-fdd66a4760eb)
\
### Variables
| *Variabel* | *Penjelasan* |
| :---------: | :--------- |
| workflow.user_complaint_summary | Summary komplain user. Digunakan untuk menangkap keluaran dari card AI Task pertama pada node complaint_summarize. String dari variabel ini nantinya akan jadi masukan bagi card Knowledge Base pada node complaint_KB jika summary komplain user telah terkonfirmasi benar |
| workflow.complaint_summ_result | Konfirmasi dari user terhadap summary yang diberikan. Merupakan masukan dari card AI Task kedua pada node complaint_summarize |
| workflow.summ_affirmationYes | Hasil klasifikasi dari kalimat konfirmasi user. Digunakan untuk menangkap keluaran dari card AI Task kedua pada node complaint_summarize. Variabel ini bertipe boolean dan merupakan hasil klasifikasi AI Task yang menentukan apakah summary dari komplain user terkonfirmasi benar atau tidak |
| turn.KnowledgeAgent.answer | Kondisi card knowledge base. Apabila card knowledge base memberikan jawaban dari dokumen, maka variabel ini akan bernilai true, jika sebaliknya maka variabel ini akan bernilai false |
\

## Sub-Flow General Question
### User intent example
### Workflow
### Variables

## Sub-Flow Fund Transfer
### User intent example
* Mau tf
* Tf dong
* Saya ingin transfer
* Mau transfer dana
* Transfer dana ke rekening bri
* Aku mau transfer uang
* Transfer ke bni
* Tf bank lain
* Transfer bang bca
* Kirim dana ke rekening lain
### Workflow
1. Mula-mula dilakukan hit API [Fund Transfer & Balance Checking Table](https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/) untuk mendapatkan data sisa saldo user, yaitu dengan script berikut:
```
const url = 'https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/get_balance/' + user.user_acctno;

try {
  const response = await axios.get(url);
  const number1 = response.data.balance;
  workflow.userBalance = number1;
} catch (error) {
  console.error('An error occurred:', error);
  workflow.identification = false
}
```
2. User akan diminta untuk memilih bank tujuan transfer. Jika bank yang dipilih adalah bank selain BRI, maka user akan diminta untuk memilih metode transfer, yaitu RTGS atau Online Transfer. User diberikan kesempatan melakukan kesalahan pengisian maksimal sebanyak 3 kali, jika kesempatan tersebut habis maka user akan kembali ke main flow\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/dafc619a-600c-40ca-93d4-0e6ebf42ba36)
3. User akan diminta untuk mengisi nomor rekening tujuan transfer. User diberikan kesempatan melakukan kesalahan pengisian maksimal sebanyak 3 kali, jika kesempatan tersebut habis maka user akan kembali ke main flow. Kesalahan pengisian dapat disebabkan oleh nomor rekening yang tidak valid, atau nomor rekening tujuan adalah nomor rekening milik user sendiri. Pengecekan terhadap kedua hal tersebut dilakukan dengan script berikut:
```
// Mengecek kevalidan nomor rekening tujuan dengan mencari ke tabel nama bank dan nomor rekening di API Fund Transfer & Balance Checking
const records = await fundTransferTable.findRecords({
  filter: {acctno: workflow.accountNumber,
            bank: workflow.bankName}
})

if (records.length > 0) {
  workflow.recipientName = records[0].fullname
  workflow.accountValid = true
} else {
  workflow.accountValid = false
}

// Mengecek apakah nomor rekening tujuan yang dimasukkan oleh user merupakan nomor rekening milik user sendiri
const records = await fundTransferTable.findRecords({
  filter: {acctno: workflow.accountNumber,
            bank: workflow.bankName}
})

if (workflow.accountNumber === user.user_acctno) {
  workflow.accountValid = false
} else {
  
}
```
4. User akan diminta untuk mengisi nominal transfer. User diberikan kesempatan melakukan kesalahan pengisian maksimal sebanyak 3 kali, jika kesempatan tersebut habis maka user akan kembali ke main flow. Kesalahan pengisian dapat disebabkan oleh nominal transfer di bawah Rp 10.000, atau saldo yang tersedia pada rekening user tidak mencukupi (sisa saldo di rekening user setelah transfer dilakukan minimal Rp 50.000). Pengecekan terhadap kedua hal tersebut dilakukan dengan script berikut:
```
// Mengecek apakah nominal transfer sudah lebih dari nominal minimal transfer
const transferNominal = workflow.transferNominal;
const nominalValid = !isNaN(transferNominal) && transferNominal >= 10000;
workflow.nominalValid = nominalValid;

// Mengecek apakah sisa saldo di rekening user setelah dilakukan transfer sudah melebihi atau sama dengan sisa saldo minimal rekening user
const a1 = parseInt(workflow.transferNominal.replace(/\D/g, ''), 10);
if (workflow.userBalance - a1 >= 50000) {
  workflow.nominalValid = true
} else {
  workflow.nominalValid = false
}
```
5. User akan diberikan rincian transfer sebelum proses transfer dilakukan
6. Jika user mengkonfirmasi rincian transfer tersebut, maka proses transfer akan dilakukan dan user akan mendapatkan bukti transfer
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/eb60d4a1-7535-4b3d-ae40-35ce7d46bac5)
### Variables

## Sub-Flow Balance Checking
### User intent example
* saldo saya sisa berapa?
* berapa sisa saldo saya?
* saya ingin tau uang saya di rekening masih ada berapa
* cek saldo
* sisa saldo
 
### Workflow
1. Saat user diarahkan ke sub-flow balance checking, akan dilakukan hit API [Fund Transfer & Balance Checking Table](https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/) menggunakan identifier berupa variable user.user_acctno untuk mendapatkan value nama dan sisa saldo di rekening user. Berikut ini adalah code snippet untuk hit API tersebut:
```
const url = 'https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/get_balance/' + user.user_acctno
const response = await axios.get(url)

const number1 = response.data.balance;
const formattedNumber1 = new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' }).format(number1);

workflow.balance = formattedNumber1
workflow.name = response.data.fullname
workflow.censoredacctno = user.user_acctno.substring(0, 4) + "*******" + user.user_acctno.substring(9)
```
2. Kemudian chatbot akan menampilkan data sisa saldo rekening user seperti pada gambar berikut:
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/e6bb9c53-870c-4ace-9ce7-3276e1b16f82)
\
Cara kerja sub-flow balance checking secara umum adalah sebagai berikut:\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/ab7b8a5b-a88e-4250-ab54-956c03409e6e)\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/5e22a1e5-adf2-46b8-9735-37227b954236)

### Variables
| *Variabel* | *Penjelasan* |
| :---------: | :--------- |
| workflow.balance | Sisa saldo user yang didapatkan dari tabel [Fund Transfer & Balance Checking Table](https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/) |
| workflow.name | Nama user yang didapatkan dari tabel [Fund Transfer & Balance Checking Table](https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/) |
| workflow.censoredacctno | Merupakan nomor rekening user, yaitu sama dengan variabel user.user_acctno namun disamarkan pada bagian tengahnya |

## Sub-Flow Score Checking
### User intent example
### Workflow
### Variables

## Sub-Flow Investment Advisor
### User intent example
User diarahkan untuk pengecekan ketersediaan data profil risiko:
* Saya punya dana sejuta, sebaiknya diinvestasikan ke mana ya?
* Saranin saya produk investasi yg bagus dong
* Aku mau investasi nih, enaknya invest ke mana ya?
* Dana seratus juta baiknya diinvestasikan ke mana?
* Saya bingung cara diversifikasi portofolio investasi, kasih rekomendasi dong
  
User langsung diarahkan untuk mengisi kuisioner profil risiko:
* Saya mau tau profil risiko saya
* Saya pengen cek profil risiko saya
* Saya ingin mengisi kuisioner profil risiko
* Isi kuisioner profil risiko
* Cek profil risiko
  
### Workflow
1. **Sebelum pengisian kuisioner profil risiko**: Pada sub-flow investment advisor, user akan diberikan rekomendasi investasi berdasarkan profil risiko mereka. Terdapat dua skenario user untuk masuk ke sub-flow ini melalui controller pada main flow, yang pertama yaitu melalui masukan-masukan user yang memiliki intensi untuk bertanya terkait rekomendasi investasi, dan yang kedua yaitu melalui masukan-masukan user yang memiliki intensi untuk melakukan pengisian kuisioner profil risiko. Klasifikasi skenario ini dilakukan oleh card AI Task pada node sub_controller. Untuk skenario yang pertama, user akan diarahkan pada node checking untuk dilakukan pengecekan data profil risiko user pada database. Jika user sebelumnya telah mengisi kuisioner profil risiko, maka user akan diberikan saran investasi berdasarkan data profil risiko user yang tersedia. Jika user belum pernah mengisi kuisioner profil risiko, maka user akan diarahkan untuk pengisian kuisioner. Sedangkan untuk skenario yang kedua, user akan langsung diarahkan untuk pengisian kuisioner profil risiko.\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/d463b822-1786-4709-aa59-3c8c530654da)
\
Pengecekan data profil risiko user dilakukan dengan melakukan hit API [Investment Risk Appetite Table](https://sabrina-investment-advisor-vq36ocpmka-et.a.run.app/) menggunakan script berikut:\
```
const url = 'https://sabrina-investment-advisor-vq36ocpmka-et.a.run.app/read_score/' + user.user_acctno;

try {
  const response = await axios.get(url);
  const number1 = response.data.riskScore;
  workflow.riskAppetite = number1;
  workflow.riskAppetiteExist = true;
} catch (error) {
  console.error('An error occured:', error);
  workflow.riskAppetiteExist = false
}
```
2. **Pengisian kuisioner profil risiko**: Terdapat 5 pertanyaan pada kuisioner profil risiko yang masing-masing jawaban user atas pertanyaan tersebut akan melalui pembobotan untuk menentukan profil risiko user. Pada saat mengisi kuisioner profil risiko, user memiliki kesempatan maksimal 3 kali untuk memilih dari pilihan jawaban yang tersedia. Jika user mengetik jawaban selain jawaban yang disediakan sebanyak 3 kali berturut-turut, maka user akan dikembalikan ke main flow.\
\
Berikut ini adalah daftar pertanyaan beserta skor masing-masing jawabannya:
```
Kuisioner:
a. Manakah yang paling menggambarkan dirimu?
- Belum menikah (20)
- Sudah menikah, belum punya anak (16)
- Sudah menikah, anak 1 (12)
- Sudah menikah, anak 2 atau lebih (8)
- Sudah pensiun (4)

b. Bagaimanakah prinsipmu dalam berinvestasi?
- Menghindari kerugian (4)
- Keduanya sama penting (12)
- Memaksimalkan keuntungan (20)

c. Jika keadaan pasar sedang tidak menentu dan nilai investasimu turun 15% dalam sebulan, apa yang akan kamu lakukan?
- Jual semua (4)
- Jual sebagian (8)
- Simpan (12)
- Beli lagi (20)

d. Bagaimana tingkat pengetahuan dan pengalaman Anda dalam berinvestasi?
- Sangat terbatas, hampir tidak tahu sama sekali (4)
- Terbatas, memiliki sedikit pemahaman dasar (8)
- Sedang, memiliki pengetahuan dasar dan pengalaman beberapa tahun (12)
- Tinggi, memiliki pengetahuan mendalam dan pengalaman yang luas (16)

e. Berapa lama rencana investasi Anda?
- Kurang dari 1 tahun (4)
- 1 hingga 3 tahun (8)
- 4 hingga 5 tahun (16)
- Lebih dari 5 tahun (24)
```
Berikut ini adalah daftar pembagian profil risiko beserta produk investasi yang disarankan:
```
a. Konservatif (Skor 0-44)
- Britama Rencana, yaitu tabungan investasi dengan setoran tetap bulanan yang dilengkapi dengan fasilitas perlindungan asuransi jiwa. Anda bisa mendaftar Britama Rencana dengan mengunjungi kantor BRI terdekat.
- Deposito, yaitu simpanan berjangka yang hanya dapat ditarik pada jangka waktu tertentu dengan bunga lebih besar dibanding giro atau tabungan. Anda bisa mendaftar Deposito Rupiah secara online di https://eform.bri.co.id/home/syarat/deposito atau dengan mengunjungi kantor cabang BRI terdekat.

b. Konservatif-Moderat (Skor 45-55)
- BRIFINE DPLK Pasar Uang, yaitu produk investasi yang memiliki tingkat risiko rendah dengan instrumen investasi sebagai berikut: Deposito, SBI, Obligasi jangka n < 1 tahun. Pendaftaran dapat dilakukan di Kantor Cabang & Kantor Cabang Pembantu BRI yang telah memiliki sistem BRIVEST.
- BRIFINE DPLK Pasar Uang Syariah, yaitu produk investasi yang memiliki tingkat risiko rendah dengan instrumen investasi yaitu: Deposito Bank Syariah, SBI Syariah, SUKUK dengan jangka waktu n < 1 tahun. Pendaftaran dapat dilakukan di Kantor Cabang & Kantor Cabang Pembantu BRI yang telah memiliki sistem BRIVEST.
- Reksa Dana Pasar Uang, yaitu instrumen investasi Efek bersifat Utang dan bertujuan untuk menjaga likuiditas dengan risiko tergolong rendah dan waktu jatuh tempo pendek, kurang lebih 1 (satu) tahun. Anda bisa membeli Reksa Dana Pasar Uang di Sentra Layanan BRI Prioritas terdekat.
- Obligasi Negara Ritel (ORI), yaitu surat berharga yang diterbitkan oleh pemerintah untuk investor ritel. ORI menawarkan pengembalian tetap (fixed rate) yang artinya tingkat kupon tidak akan berubah sampai jatuh tempo. Kamu dapat melakukan pemesanan melalui SBN Online BRI di https://sbn.bri.co.id.

c. Moderat (Skor 56-63)
- BRIFINE DPLK Pendapatan Tetap, yaitu produk investasi yang memiliki tingkat risiko sedang dengan instrumen investasi sebagai berikut: SUKUK, Surat Berharga Negara, Obligasi Korporasi BUMN. Pendaftaran dapat dilakukan di Kantor Cabang & Kantor Cabang Pembantu BRI yang telah memiliki sistem BRIVEST.
- Reksa Dana Pendapatan Tetap, yaitu produk reksadana yang sebagian besar alokasi dananya berbentuk efek utang, dengan risiko lebih besar dari Pasar Uang dan waktu jatuh tempo  lebih dari 1 (satu) tahun.Anda bisa membeli Reksa Dana Pendapatan Tetap di Sentra Layanan BRI Prioritas terdekat.

d. Moderat-Agresif (Skor 64-71)
- BRIFINE DPLK Kombinasi, yaitu produk investasi yang memiliki tingkat risiko sesuai komposisi dengan instrumen investasi berupa kombinasi antara 2 atau 3 investasi sesuai pilihan peserta. Pendaftaran dapat dilakukan di Kantor Cabang & Kantor Cabang Pembantu BRI yang telah memiliki sistem BRIVEST.

e. Agresif (Skor 72-100)
- BRIFINE DPLK Saham, yaitu produk investasi yang memiliki tingkat risiko tinggi dengan instrumen investasi sebagai berikut: Reksadana Saham, Saham. Pendaftaran dapat dilakukan di Kantor Cabang & Kantor Cabang Pembantu BRI yang telah memiliki sistem BRIVEST.
- Reksa Dana Saham, yaitu produk investasi yang menempatkan investasi ke pembelian saham-saham yang tercatat di Bursa Efek Indonesia, dengan risiko lebih tinggi (dari pasar uang dan pendapatan tetap) dan pengembalian yang lebih tinggi. Anda bisa membeli Reksa Dana Saham di Sentra Layanan BRI Prioritas terdekat.
- Rekening Dana Nasabah (RDN), yaitu rekening yang dipergunakan khusus untuk Anda yang ingin bertransaksi efek (Saham/Obligasi) di Bursa Efek Indonesia. Anda dapat melakukan pembukaan RDN melalui aplikasi BRImo.
```
3. **Setelah pengisian kuisioner profil risiko**: Hasil pengisian kuisioner profil risiko oleh user akan ditambahkan ke dalam tabel skor profil risiko user. Jika user sebelumnya telah mengisi kuisioner profil risiko, maka skor profil risiko user akan diupdate. Jika user belum pernah mengisi kuisioner profil risiko, maka skor profil risiko user akan ditambahkan pada row baru. Penambahan atau update tabel skor profil risiko user ini dilakukan dengan script berikut:
```
const url1 = 'https://sabrina-investment-advisor-vq36ocpmka-et.a.run.app/update_score/' + user.user_acctno + '/' + x;
const url2 = 'https://sabrina-investment-advisor-vq36ocpmka-et.a.run.app/create_score/' + user.user_acctno + '/' + x;

try {
  const response = await axios.get(url1);
  const data = response.data;
  workflow.riskAppetite = data.riskScore;
  workflow.riskAppetiteExist = true;
} catch (error) {
  const response2 = await axios.get(url2);
  const data2 = response2.data;
  workflow.riskAppetite = data2.riskScore;
  console.error('An error occurred:', error);
  workflow.riskAppetiteExist = false;
}
```
\
Cara kerja sub-flow investment advisor secara umum adalah sebagai berikut:\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/b85da0c3-1a95-48d2-ae7c-5625c99e414a)
### Variables

## Sub-Flow Location Information
### User intent example
### Workflow
### Variables

## Documents, Tables, & APIs
### Product Knowledge Document
### Complaint Handling Knowledge Document
### [Fund Transfer & Balance Checking Table](https://sabrina-fund-transfer-vq36ocpmka-et.a.run.app/)
### [Score Checking Table](https://sabrina-credit-scoring-vq36ocpmka-et.a.run.app/)
### [Investment Risk Appetite Table](https://sabrina-investment-advisor-vq36ocpmka-et.a.run.app/)
### [Google Maps API](https://developers.google.com/maps)
### [Mozilla Speech Recognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition)
