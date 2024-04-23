# Projeto-Sistema-de-Estoque-Front-end
//Front-end

//Criando o projeto React em um novo terminal 

npx create-react-app sistema-estoque-frontend
cd sistema-estoque-frontend


//Instalação de Dependências

Instalando a dependência Axios para fazer requisições HTTP

npm install axios


//Configurando a interface do usuario 

// src/App.js

import React, { useState, useEffect } from 'react';
import axios from 'axios';

function App() {
  const [products, setProducts] = useState([]);
  const [productName, setProductName] = useState('');
  const [productQuantity, setProductQuantity] = useState(0);

  useEffect(() => {
    fetchProducts();
  }, []);

  const fetchProducts = async () => {
    try {
      const response = await axios.get('http://localhost:3000/api/products');
      setProducts(response.data);
    } catch (error) {
      console.error('Erro ao buscar produtos:', error);
    }
  };

  const handleAddProduct = async () => {
    try {
      await axios.post('http://localhost:3000/api/products', {
        name: productName,
        quantity: productQuantity
      });
      fetchProducts();
      setProductName('');
      setProductQuantity(0);
    } catch (error) {
      console.error('Erro ao adicionar produto:', error);
    }
  };

  const handleDeleteProduct = async (productId) => {
    try {
      await axios.delete(`http://localhost:3000/api/products/${productId}`);
      fetchProducts();
    } catch (error) {
      console.error('Erro ao remover produto:', error);
    }
  };

  return (
    <div className="App">
      <h1>Sistema de Estoque</h1>
      <div>
        <input
          type="text"
          placeholder="Nome do Produto"
          value={productName}
          onChange={(e) => setProductName(e.target.value)}
        />
        <input
          type="number"
          placeholder="Quantidade"
          value={productQuantity}
          onChange={(e) => setProductQuantity(e.target.value)}
        />
        <button onClick={handleAddProduct}>Adicionar Produto</button>
      </div>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            {product.name} - Quantidade: {product.quantity}{' '}
            <button onClick={() => handleDeleteProduct(product.id)}>
              Remover
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;


//Executando Front-end 

npm start

