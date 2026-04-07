# lista-app
import { useState } from 'react';
import {
  View, Text, TextInput, TouchableOpacity,
  FlatList, StyleSheet
} from 'react-native';

export default function App() {
  const [produto, setProduto] = useState('');
  const [preco, setPreco] = useState('');
  const [lista, setLista] = useState([]);

  function adicionarVenda() {
    if (produto === '' || preco === '') return;

    const novaVenda = {
      id: Date.now().toString(),
      nome: produto,
      valor: parseFloat(preco)
    };

    setLista([...lista, novaVenda]);
    setProduto('');
    setPreco('');
  }

  function removerVenda(id) {
    setLista(lista.filter(item => item.id !== id));
  }

  function totalVendas() {
    return lista.reduce((total, item) => total + item.valor, 0);
  }

  return (
    <View style={styles.container}>

      {/* Título */}
      <Text style={styles.titulo}>💰 Vendas</Text>

      {/* Inputs */}
      <View style={styles.card}>
        <TextInput
          style={styles.input}
          placeholder="Produto"
          value={produto}
          onChangeText={setProduto}
        />

        <TextInput
          style={styles.input}
          placeholder="Preço"
          keyboardType="numeric"
          value={preco}
          onChangeText={setPreco}
        />

        <TouchableOpacity style={styles.botao} onPress={adicionarVenda}>
          <Text style={styles.botaoTexto}>Adicionar</Text>
        </TouchableOpacity>
      </View>

      {/* Resumo */}
      <View style={styles.resumoBox}>
        <Text style={styles.resumo}>Itens: {lista.length}</Text>
        <Text style={styles.resumo}>
          Total: R$ {totalVendas().toFixed(2)}
        </Text>
      </View>

      {/* Lista */}
      <FlatList
        data={lista}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.item}>
            <View>
              <Text style={styles.itemNome}>{item.nome}</Text>
              <Text style={styles.itemPreco}>
                R$ {item.valor.toFixed(2)}
              </Text>
            </View>

            <TouchableOpacity
              style={styles.btnDelete}
              onPress={() => removerVenda(item.id)}
            >
              <Text style={styles.deleteTexto}>X</Text>
            </TouchableOpacity>
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f2f2f2',
    padding: 20,
    marginTop: 40
  },

  titulo: {
    fontSize: 28,
    fontWeight: 'bold',
    marginBottom: 15
  },

  card: {
    backgroundColor: '#fff',
    padding: 15,
    borderRadius: 10,
    marginBottom: 15
  },

  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    borderRadius: 8,
    marginBottom: 10
  },

  botao: {
    backgroundColor: '#4CAF50',
    padding: 12,
    borderRadius: 8,
    alignItems: 'center'
  },

  botaoTexto: {
    color: '#fff',
    fontWeight: 'bold'
  },

  resumoBox: {
    marginBottom: 10
  },

  resumo: {
    fontSize: 16,
    fontWeight: '500'
  },

  item: {
    backgroundColor: '#fff',
    padding: 15,
    borderRadius: 10,
    marginTop: 10,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center'
  },

  itemNome: {
    fontSize: 16,
    fontWeight: 'bold'
  },

  itemPreco: {
    color: 'green'
  },

  btnDelete: {
    backgroundColor: 'red',
    padding: 10,
    borderRadius: 8
  },

  deleteTexto: {
    color: '#fff',
    fontWeight: 'bold'
  }
});
