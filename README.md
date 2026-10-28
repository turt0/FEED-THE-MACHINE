# FEED THE MACHINE

Projeto para as A3s das UCs de Computação Gráfica e Teoria da Compilação.

Um jogo de quebra-cabeça educativo no estilo Game Boy Advance. Você é John Characterman, funcionário novo de uma corporação brutalista em que cada andar do prédio é uma fase de um compilador. Esta demo tem o Andar 1, o Departamento de Triagem (análise léxica), e o final no Andar 6, onde você alimenta o disquete final para THE BOSS. Os andares 2 a 5 ainda estão em construção: depois do relatório do Andar 1, o elevador sobe direto para o topo.

## Como jogar

Baixe o arquivo `feed-the-machine.html` e abra no navegador (Chrome, Edge, Firefox). Não precisa instalar nada nem rodar servidor: o jogo inteiro, incluindo as imagens, está nesse único arquivo.

O som começa depois da primeira tecla, então aperte Enter na tela de título.

## Controles

| Tecla | Ação |
| --- | --- |
| Setas / WASD | Andar (um toque rápido só vira o personagem) |
| Z | Pegar, entregar, interagir, avançar o texto |
| X | Abrir a prancheta com a ordem do dia |
| Enter | Começar / pausar |
| F11 | Tela cheia |
| F12 | Salvar um screenshot (`screenshot_000.png`) |

## O Andar 1: análise léxica

A ordem do dia é uma linha de código:

```
res = a1+b2*10; // sum
```

Ela passa por um scanner de verdade (maximal munch), que corta a linha em lexemas. Cada lexema chega numa caixa pela esteira, e o jogador leva a caixa até a calha certa:

| Calha | O que vai nela |
| --- | --- |
| ID | Nomes: começam com letra e podem ter dígitos (`res`, `a1`) |
| NUM | Só dígitos (`10`) |
| OP | Operadores (`=` `+` `-` `*` `/`) |
| SYM | Pontuação (`;` `(` `)` `,`) |
| TRASH | Espaços e comentários, que nunca viram tokens |

Cada caixa entregue na calha certa vira um token. Cada erro dá uma advertência, e na terceira o funcionário é demitido (com lança-chamas). Ao final, o andar entrega o fluxo de tokens para o andar seguinte:

```
<ID,RES> <OP,=> <ID,A1> <OP,+> <ID,B2> <OP,*> <NUM,10> <SYM,;>
```

## Licença

MIT. Veja [LICENSE](LICENSE).
