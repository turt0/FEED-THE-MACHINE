# FEED THE MACHINE

Projeto para as A3s das UCs de Computação Gráfica e Teoria da Compilação.

Um jogo de quebra-cabeça educativo no estilo Game Boy Advance. Você é John Characterman, funcionário novo de uma corporação brutalista em que cada andar do prédio é uma fase de um compilador. Esta demo tem o Andar 1, o Departamento de Triagem (análise léxica), e o final no Andar 6, onde você alimenta o disquete final para THE BOSS. Os andares 2 a 5 ainda estão em construção: depois do relatório do Andar 1, o elevador sobe direto para o topo.

## Como jogar

Baixe o arquivo `feed-the-machine.html` e abra no navegador (Chrome, Edge, Firefox). Não precisa instalar nada nem rodar servidor: o jogo inteiro, incluindo as imagens, está nesse único arquivo.

O som começa depois da primeira tecla (regra dos navegadores), então aperte Enter na tela de título.

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

## Computação gráfica

O jogo não usa engine nem bibliotecas gráficas. Tudo é desenhado pixel a pixel num framebuffer de cores indexadas de 240×160, com uma paleta de 5 cores:

- **Primitivas próprias:** retângulos, linhas com o algoritmo de Bresenham e o arco do mostrador do elevador com o algoritmo do ponto médio para círculos.
- **Fonte bitmap 3×5** feita à mão, com quebra de linha.
- **Sprites:** os PNGs desenhados no Aseprite são convertidos na carga, e cada pixel vira a cor mais próxima da paleta (quantização por distância euclidiana no espaço RGB).
- **Cenário procedural:** concreto, paredes e manchas gerados por um hash determinístico.
- **Efeitos:** fade entre cenas, tremor de tela e partículas com velocidade e gravidade.
- **Som:** um sintetizador por software gera ondas quadrada, dente de serra, triangular e ruído, amostra por amostra.

O navegador só entra no fim do processo: mostra o framebuffer num `<canvas>`, ampliado por um fator inteiro para os pixels ficarem nítidos, lê o teclado e toca o áudio. O jogo roda num loop fixo de 60 atualizações por segundo, independente da taxa de atualização do monitor.

## Organização do código

O `feed-the-machine.html` é uma conversão para JavaScript de uma versão em C++17 com SDL2. O script está dividido em seções, cada uma correspondendo a um módulo da versão original:

| Seção | O que faz |
| --- | --- |
| `core/` | Canvas e paleta, fonte, sprites, entrada, sintetizador, utilitários |
| `compiler/Lexer` | O scanner, independente do jogo |
| `game/` | Gerenciador de cenas e fades, caixa de diálogo, efeitos sonoros |
| `game/scenes/` | Título, elevador, relatório do andar, tela final |
| `game/floor/` | O que todo andar tem: funcionário, HUD, advertências, prancheta, pausa, elevador |
| `game/floor1/` | Andar 1: esteira, caixas, calhas, triturador e as regras de triagem |
| `game/floor6/` | Andar 6: a sequência do Boss, uma pequena máquina de estados |
| `platform/` | A única parte que fala com o navegador: canvas, teclado, áudio e o loop principal |

## Licença

MIT. Veja [LICENSE](LICENSE).
