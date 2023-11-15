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

Pada Sabrina BotPress terdapat beberapa komponen lain seperti:
* [Documents for RAG](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#rag-in-botpress)
* [Tables](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#tables)

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
Secara garis besar, cara kerja main flow adalah sebagai berikut:\
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/572e4890-9c08-4a7b-9f75-5d935e178e34)

## Sub-Flow Product Information
### User's possible intents
### Workflow
### Variables

## Sub-Flow Complaint Handling
### User's possible intents
### Workflow
### Variables

## Sub-Flow General Question
### User's possible intents
### Workflow
### Variables

## Sub-Flow Fund Transfer
### User's possible intents
### Workflow
### Variables

## Sub-Flow Balance Checking
### User's possible intents
### Workflow
### Variables

## Sub-Flow Score Checking
### User's possible intents
### Workflow
### Variables

## Sub-Flow Investment Advisor
### User's possible intents
### Workflow
### Variables

## Sub-Flow Location Information
### User's possible intents
### Workflow
### Variables

## RAG in BotPress
### Product Information Document
### Complaint Handling Document

## Tables & APIs
### Fund Transfer Table
### Score Checking Table
### Location API
