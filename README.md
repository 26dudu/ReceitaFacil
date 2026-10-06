# ReceitaFácil 🍳

**Autor:** Eduardo
**Atividade:** Missão de Navegação com Jetpack Compose (padrões de navegação do HabitTracker aplicados a um novo tema)

## Tema escolhido

**Catálogo de Receitas** — um app para consultar receitas, marcar favoritas e ver ingredientes e modo de preparo.

## Telas implementadas

Fluxo: `Splash → (condicional) → Login ou Painel Principal (Bottom Navigation, 3 abas) → Lista → Detalhe`

| Tela | Descrição |
|------|-----------|
| **Splash** | Tela de abertura (1,5 s). Só avisa que terminou (`aoTerminar`); **não decide** o destino. |
| **Login** | Formulário de e-mail e senha. Ao entrar, vai para o catálogo. |
| **Receitas** (aba 1) | Lista com o catálogo de receitas. Mostra mensagem de **estado vazio** quando todas são removidas, com botão para restaurar. |
| **Favoritas** (aba 2) | Lista apenas das receitas favoritadas, com estado vazio próprio. |
| **Perfil** (aba 3) | Contadores, botão "Restaurar catálogo" e "Sair da conta". |
| **Detalhe** | Ingredientes, modo de preparo, favoritar e remover. Recebe `receitaId` por rota tipada. |

## Como os requisitos foram atendidos

- **Rotas tipadas:** `model/Rota.kt` declara uma `sealed interface Rota` com `@Serializable` em todos os destinos. Nenhuma rota é `String` solta.
- **Navegação linear:** a Splash segue sozinha para a próxima tela após o `delay`.
- **Navegação condicional:** o `if (viewModel.estaLogado)` fica em `AppNavigation`, que chama a Splash. A Splash apenas executa o callback.
- **Parâmetro type-safe:** a lista navega com `Rota.DetalheReceita(receitaId)` e o destino lê com `backStackEntry.toRoute()`.
- **Bottom Navigation persistente:** `navegarParaAba()` em `PainelPrincipal.kt` usa `saveState`, `restoreState` e `launchSingleTop` juntos, preservando a rolagem ao trocar de aba.
- **ViewModel único:** `ReceitaViewModel` é criado uma única vez em `AppNavigation` (que hospeda o `NavHost`) e passado por parâmetro. Nenhuma tela chama `viewModel()`.
- **Paleta própria:** `Tema.kt` define `lightColorScheme(...)`. É o único arquivo com `Color(0xFF...)`.
- **Estado vazio:** componente `EstadoVazio` usado nas listas de Receitas e Favoritas.
- **Acessibilidade:** todo `Icon` sem texto ao lado tem `contentDescription` descrevendo a ação.

## Estrutura do projeto

```
app/src/main/java/com/receitafacil/app/
├── MainActivity.kt
├── model/          Receita.kt, Rota.kt
├── viewmodel/      ReceitaViewModel.kt
└── userinterface/  AppNavigation.kt, PainelPrincipal.kt, SplashScreen.kt,
                    LoginScreen.kt, ListaReceitasScreen.kt, FavoritasScreen.kt,
                    PerfilScreen.kt, DetalheReceitaScreen.kt,
                    ComponentesReceita.kt, Tema.kt
```
