## Page-e-Paeable
A lista `Page` e o argumento `Pageable` trabalham juntos para **permitir que você divida os dados em páginas** (paginação), facilitando o acesso a grandes quantidades de dados de forma ordenada e segmentada. Vou explicar cada um detalhadamente e como eles funcionam juntos:

### 1. O que é `Page`?

- **`Page`** é um tipo de estrutura especial para dados paginados, que contém:
  - Uma lista de dados da página atual (por exemplo, a primeira página de produtos).
  - Informações sobre a paginação, como o número da página atual, o tamanho da página (quantos itens por página), o número total de páginas e o total de itens.
- Pense na `Page` como uma **"caixa" com uma lista de itens** (os dados da página) e informações sobre quantas "caixas" (páginas) existem no total.

### 2. O que é `Pageable`?

- **`Pageable`** é a **configuração que define como você quer dividir os dados em páginas**. Ele contém:
  - **Número da página**: Qual página você quer acessar (por exemplo, a segunda página).
  - **Tamanho da página**: Quantos itens devem aparecer em cada página (por exemplo, 10 itens por página).
  - **Ordenação** (opcional): Em que ordem você quer que os dados apareçam (por exemplo, ordenados pelo nome do produto).
- `Pageable` é passado como argumento porque ele **informa ao banco de dados como dividir e ordenar os dados** ao fazer a consulta.

### 3. Como `Pageable` e `Page` Trabalham Juntos no Código

Quando você chama um método como `repository.findAll(pageable)`, ele usa o `Pageable` para:
1. **Dividir os dados em páginas**, baseado nas configurações de `Pageable`.
2. **Retornar uma `Page`** contendo:
   - Os itens da página atual.
   - Informações sobre a paginação (por exemplo, número da página, total de páginas).

Exemplo:

```java
public Page<ProductDTO> findAll(Pageable pageable) {
    Page<Product> result = repository.findAll(pageable); // Consulta paginada
    return result.map(ProductDTO::new); // Converte os dados da página para ProductDTO e retorna
}
```

- Aqui, `repository.findAll(pageable)` retorna uma `Page<Product>`.
- `result.map(ProductDTO::new)` transforma cada `Product` da página em `ProductDTO`.
- O método devolve uma `Page<ProductDTO>`, que ainda é uma estrutura de página, mas agora contendo `ProductDTO`s em vez de `Product`s.

### 4. Por Que Usar `Pageable`?

Usar `Pageable` é útil porque:
- **Evita sobrecarregar o sistema** ao lidar com grandes volumes de dados.
- **Melhora a experiência do usuário**, pois ele pode navegar entre páginas de dados (por exemplo, navegando por uma lista de produtos, 10 por vez).
- **Permite controle flexível** de qual página acessar e quantos itens incluir.

Em resumo:
- `Pageable` define como dividir e ordenar os dados.
- `Page` é a estrutura que contém a página específica de dados e informações sobre a paginação. 

Essa combinação permite carregar apenas partes dos dados, o que é especialmente útil para melhorar o desempenho e a navegação em grandes conjuntos de dados.
