---
icon: lucide/rocket
---

# Exemplos

Para a documentação completa visite [zensical.org](https://zensical.org/docs/).

## Comandos

* [`zensical new`][new] - Criar um novo projeto
* [`zensical serve`][serve] - Iniciar servidor web local
* [`zensical build`][build] - Compilar seu site

  [new]: https://zensical.org/docs/usage/new/
  [serve]: https://zensical.org/docs/usage/preview/
  [build]: https://zensical.org/docs/usage/build/

## Exemplos

### Admonições

> Vá para a [documentação](https://zensical.org/docs/authoring/admonitions/)

!!! note

    Esta é uma admonição de **nota**. Use-a para fornecer informações úteis.

!!! warning

    Esta é uma admonição de **aviso**. Tenha cuidado!

### Detalhes

> Vá para a [documentação](https://zensical.org/docs/authoring/admonitions/#collapsible-blocks)

??? info "Clique para expandir para mais informações"
    
    Este conteúdo está oculto até que você clique para expandi-lo.
    Ótimo para FAQs ou longas explicações.

## Blocos de Código

> Vá para a [documentação](https://zensical.org/docs/authoring/code-blocks/)

``` python hl_lines="2" title="Blocos de código"
def greet(name):
    print(f"Olá, {name}!") # (1)!

greet("Python")
```

1.  > Vá para a [documentação](https://zensical.org/docs/authoring/code-blocks/#code-annotations)

    Anotações de código permitem anexar notas a linhas de código.

Código também pode ser destacado em linha: `#!python print("Olá, Python!")`.

## Abas de Conteúdo

> Vá para a [documentação](https://zensical.org/docs/authoring/content-tabs/)

=== "Python"

    ``` python
    print("Olá do Python!")
    ```

=== "Rust"

    ``` rs
    println!("Olá do Rust!");
    ```

## Diagramas

> Vá para a [documentação](https://zensical.org/docs/authoring/diagrams/)

``` mermaid
graph LR
  A[Início] --> B{Erro?};
  B -->|Sim| C[Hmm...];
  C --> D[Depurar];
  D --> B;
  B ---->|Não| E[Eba!];
```

## Notas de Rodapé

> Vá para a [documentação](https://zensical.org/docs/authoring/footnotes/)

Aqui está uma frase com uma nota de rodapé.[^1]

Passe o mouse para ver uma dica.

[^1]: Esta é a nota de rodapé.


## Formatação

> Vá para a [documentação](https://zensical.org/docs/authoring/formatting/)

- ==Isto foi marcado (destaque)==
- ^^Isto foi inserido (sublinhado)^^
- ~~Isto foi excluído (tachado)~~
- H~2~O
- A^T^A
- ++ctrl+alt+del++

## Ícones, Emojis

> Vá para a [documentação](https://zensical.org/docs/authoring/icons-emojis/)

* :sparkles: `:sparkles:`
* :rocket: `:rocket:`
* :tada: `:tada:`
* :memo: `:memo:`
* :eyes: `:eyes:`

## Matemática

> Vá para a [documentação](https://zensical.org/docs/authoring/math/)

$$
\cos x=\sum_{k=0}^{\infty}\frac{(-1)^k}{(2k)!}x^{2k}
$$

!!! warning "Necessita configuração"
    Note que o MathJax é incluído via uma tag `script` nesta página e não está
    configurado na configuração padrão gerada para evitar incluí-lo em
    páginas que não precisam dele. Veja a documentação para detalhes sobre como
    configurá-lo em todas as suas páginas se elas forem mais pesadas em Matemática do que estas
    páginas iniciais simples.

<script id="MathJax-script" async src="https://unpkg.com/mathjax@3/es5/tex-mml-chtml.js"></script>
<script>
  window.MathJax = {
    tex: {
      inlineMath: [["\\(", "\\)"]],
      displayMath: [["\\[", "\\]"]],
      processEscapes: true,
      processEnvironments: true
    },
    options: {
      ignoreHtmlClass: ".*|",
      processHtmlClass: "arithmatex"
    }
  };
</script>

## Listas de Tarefas

> Vá para a [documentação](https://zensical.org/docs/authoring/lists/#using-task-lists)

* [x] Instalar Zensical
* [x] Configurar `zensical.toml`
* [x] Escrever documentação incrível
* [ ] Implantar em qualquer lugar

## Dicas de Ferramenta (Tooltips)

> Vá para a [documentação](https://zensical.org/docs/authoring/tooltips/)

[Passe o mouse em mim][example]

  [example]: https://example.com "Eu sou uma dica!"
