# Sabrina BotPress
Sabrina BotPress merupakan customer-facing chatbot milik BRI yang memadukan sistem chatbot rule-based dengan kapabilitas LLM serta Retrieval-Augmented Generation (RAG). Chatbot ini memiliki beragam fitur, yaitu:
* [Main Flow](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#main-flow)
* [Product Information](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-product-information)
* [Complaint Handling](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-complaint-handling)
* [General Question](https://github.com/fafafwzn/sabrinaPressDocs/blob/main/README.md#sub-flow-general-question)
* Fund Transfer
* Balance Checking
* Score Checking
* Investment Advisor
* Location Information

Pada Sabrina BotPress terdapat beberapa komponen lain seperti:
* Documents for RAG
* Tables

## Platform Overview
### Nodes & Cards
Node: Sebuah elemen yang apabila ter-trigger akan mengeksekusi card yang berada di dalamnya secara berurutan dari atas hingga ke bawah. Node memiliki 1 input dan 1 atau lebih output yang akan ter-trigger apabila kondisi tertentu terpenuhi. Untuk memastikan node memiliki keluaran, letakkan card "Expression" dengan condition "true" di bagian paling akhir dari suatu node.\
\
Card: Elemen yang lebih kecil daripada node dan berperilaku sesuai dengan jenisnya.
* Intent: Mentrigger output dari node apabila chatbot menerima pesan dari user yang memiliki suatu intent tertentu yang telah didefinisikan pada bagian pengaturan dari card tersebut
* Expression: Mentrigger output dari node apabila suatu kondisi yang didefinisikan dalam codingan pada bagian Condition di pengaturan card bernilai "true". Best practice dari card ini adalah untuk mengkombinasikan dengan variable, kemudian menuliskan kondisi yang diinginkan pada kolom "Label", kemudian Generative AI akan secara otomatis menuliskan codingan yang sesuai dengan kondisi yang kita inginkan di bagian Condition
* AI Task: "Task Instructions" diisikan prompt template berisi perintah kita. "AI Task Input" diisikan masukan, best practicenya diisi dengan variabel {{event.preview}} jika inputnya adalah pesan terbaru dari user, atau dapat juga diset ke variabel tertentu apabila masukannya adalah keluaran dari proses yang lain. "Store result in variables" diisikan variabel yang ingin kita influence dengan AI Task ini. Bisa kita isikan variabel yang menangkap keluaran dari Generative AI, atau bisa juga kita berikan variable boolean yang bisa dialter oleh Generative AI sesuai dengan hasil klasifikasinya (jika kita berikan Task Instructions untuk mengklasifikasikan). "Example Input" berisi example few-shot, cara yang baik untuk cheating jawaban
* Execute Code: Kita dapat menuliskan script dalam Javascript ke dalam execute code ini. Best practicenya adalah untuk memanipulasi nilai atau tipe data dari variabel tertentu, membuat logic if-else atau looping, dan melakukan hit API tertentu jika suatu kondisi terpenuhi
* Raw Input: Berguna untuk menangkap masukan dari user. Pada bagian pengaturan, di bagian "Type of value to extract" kita dapat mensetting tipe data yang akan diterima dari pesan user, di bagian "Question to ask to the user" kita menuliskan pertanyaan yang akan ditanyakan kepada user saat meminta mereka untuk mengetikkan sesuatu, kemudian di bagian "Store result in" dapat kita isikan variabel tempat kita menyetorkan masukan dari user (variabel ini akan memiliki tipe data yang sesuai dengan pilihan kita pada bagian "Type of value to extract"). Raw input juga dapat diatur lebih lanjut dengan mengeklik tombol "Advanced Configuration". Pengaturan lanjutan disediakan yaitu "Validation" untuk melakukan validasi terhadap input user yang mana validasi tersebut dilakukan berdasarkan pencocokan dengan kondisi yang diatur melalui script validator. Kemudian "Number of retries" untuk mensetting jumlah maksimal kesalahan pengisian oleh user. Kemudian "Extract from history" untuk mengikutsertakan pesan-pesan sebelumnya ke dalam variabel (history yang terbawa maksimal hanya 5 pesan sebelumnya). Kemudian "Choice" untuk menyediakan pilihan ganda untuk dipilih oleh user, jika nantinya user memilih salah satu dari pilihan yang disediakan, maka user inputnya akan berupa pilihan user tersebut
* Triggers: Pemicu dimulainya percakapan dari node tertentu. Dapat dipilih menggunakan pemicu "Conversation Started" atau "Custom Trigger (advanced)" yang pemicunya adalah adanya payload yang masuk (dapat diakses di variabel ({{event.payload}})

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
1. Pada setiap awal session, user akan disambut dengan sapaan pembuka dari Sabrina.
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/153d654d-4f8c-4770-ac4e-c30822788832)
Di tahap ini, user bisa memilih topik yang disediakan atau mengetik langsung pertanyaan mereka.
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/38fd56ae-f655-4da6-b931-589e298af39c)
> Jika user memilih topik "Informasi Produk", "Komplain", atau "Pertanyaan Umum BRI", maka mereka akan diarahkan ke node
![image](https://github.com/fafafwzn/sabrinaPressDocs/assets/44219042/8071e806-645d-4c55-b4aa-45acdbb9fb32)

3. 




## Sub-Flow Product Information
### User's possible intents
### Workflow

## Sub-Flow Complaint Handling
### User's possible intents
### Workflow

## Sub-Flow General Question
### User's possible intents
### Workflow

## RAG in BotPress
### Product Information Document
### Complaint Handling Document

## Tables
###


