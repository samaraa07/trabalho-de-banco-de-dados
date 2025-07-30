LISTAS:

import { useState, useEffect } from 'react';

export default function Listas() {
  const [idDoFiltro, setIdDoFiltro] = useState('');
  const [postsFiltrados, setPostsFiltrados] = useState([]);
  const [estaCarregando, setEstaCarregando] = useState(false);
  const [erro, setErro] = useState(null);
  const [atualizar, setAtualizar] = useState(0);

  useEffect(() => {
    const buscarPosts = async () => {
      setEstaCarregando(true);
      setErro(null);

      try {
        const resposta = await fetch('https://jsonplaceholder.typicode.com/albums');
        if (!resposta.ok) throw new Error(Erro: ${resposta.status});

        const dados = await resposta.json();
        const filtrados = idDoFiltro === ''
          ? dados
          : dados.filter(post => post.userId === Number(idDoFiltro));

        setPostsFiltrados(filtrados);
      } catch (erro) {
        setErro(erro.message);
      } finally {
        setEstaCarregando(false);
      }
    };

    buscarPosts();
  }, [idDoFiltro, atualizar]);

  return (
    <div>
      <h1>Álbuns</h1>

      <input
        type="number"
        placeholder="ID do usuário (Não digite mais que 10)"
        value={idDoFiltro}
        onChange={(e) => setIdDoFiltro(e.target.value)}
      />

      <button onClick={() => setAtualizar(a => a + 1)}>
        Atualizar Lista
      </button>

      {estaCarregando && <p>Carregando...</p>}
      {erro && <p>Erro: {erro}</p>}

      {!estaCarregando && !erro && (
        postsFiltrados.length === 0 && idDoFiltro !== '' ? (
          <p>Nenhum álbum encontrado para o usuário "{idDoFiltro}".</p>
        ) : (
          postsFiltrados.map(post => (
            <div key={post.id}>
              <h3>{post.title}</h3>
              <p>Usuário: {post.userId}</p>
            </div>
          ))
        )
      )}
    </div>
  );
}
