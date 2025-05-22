# 🛒 Lista de Compras - Android com Kotlin

Este é um aplicativo de lista de compras desenvolvido em Kotlin para Android, utilizando arquitetura MVVM e banco de dados local com Room. O app permite adicionar, visualizar e excluir itens de uma lista de compras.

## 📱 Funcionalidades

- Adicionar itens com nome e quantidade.
- Visualizar todos os itens cadastrados.
- Remover itens da lista com um clique.
- Armazenamento persistente com Room Database.
- Interface moderna com Material Design.

## 🧠 Arquitetura

O app segue o padrão **MVVM (Model - View - ViewModel)**, promovendo uma separação clara entre lógica de UI, dados e regras de negócio.

---

## 🧩 Componentes principais

### 📌 ItemModel.kt

Define a estrutura de um item da lista:

```kotlin
@Entity(tableName = "item_table")
data class ItemModel(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val nome: String,
    val quantidade: Int
)
```
🗃️ ItemDao.kt
Interface que define operações no banco de dados:

```@Dao
interface ItemDao {
    @Insert
    fun insert(item: ItemModel)

    @Delete
    fun delete(item: ItemModel)

    @Query("SELECT * FROM item_table")
    fun getAll(): LiveData<List<ItemModel>>
}```

🧠 ItemsViewModel.kt
Controla o fluxo de dados entre a UI e o banco:

```class ItemsViewModel(application: Application): AndroidViewModel(application) {
    private val dao = ItemDatabase.getDatabase(application).itemDao()
    val items: LiveData<List<ItemModel>> = dao.getAll()

    fun insert(item: ItemModel) {
        viewModelScope.launch(Dispatchers.IO) {
            dao.insert(item)
        }
    }

    fun delete(item: ItemModel) {
        viewModelScope.launch(Dispatchers.IO) {
            dao.delete(item)
        }
    }
}```

🎛️ MainActivity.kt
Tela principal com lógica de exibição e ações:

```val viewModel = ViewModelProvider(this).get(ItemsViewModel::class.java)

viewModel.items.observe(this) { lista ->
    adapter.submitList(lista)
}

binding.btnAdicionar.setOnClickListener {
    val nome = binding.editNome.text.toString()
    val quantidade = binding.editQuantidade.text.toString().toIntOrNull() ?: 1
    val item = ItemModel(nome = nome, quantidade = quantidade)
    viewModel.insert(item)
}```



🧱 Tecnologias utilizadas

Kotlin
Room (persistência local)
ViewModel + LiveData
RecyclerView
Material Components
Data Binding


🛠️ Como rodar o projeto
Clone o repositório:

bash
Copiar
Editar
```git clone https://github.com/seuusuario/android-lista-de-compras.git```
Abra no Android Studio.

Execute em um emulador ou dispositivo Android.

📸 Telas


escreva o produto que deseja adicionar a lista
![image](https://github.com/user-attachments/assets/61a14d8d-e3a6-401b-bdab-78a456abb3ea)

ao clicar no botão, o item é adicionado e pode ser removido ao clicar no ícone da lixeira
![image](https://github.com/user-attachments/assets/0a6fcbf8-9440-4017-a1e2-c2a8b71ea9e6)


