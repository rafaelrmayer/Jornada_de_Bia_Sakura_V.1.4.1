# Jornada de Bia 🌻
### Tecnologia assistiva, comunicação alternativa e aumentativa (CAA) e inclusão

**A Jornada de Bia é um projeto de tecnologia assistiva voltado a apoiar a comunicação, a autonomia e a expressão de pessoas que se comunicam de maneiras diferentes.** O projeto reúne uma interface visual acessível, cartões e símbolos de comunicação, preferências de aparência e recursos locais de apoio ao uso cotidiano.

> “Não é sobre ensinar alguém a falar. É sobre garantir que sua voz possa ser ouvida.”

A proposta parte de um princípio simples: toda pessoa merece oportunidades para expressar sentimentos, necessidades, escolhas, vontades e pensamentos. A comunicação não precisa acontecer de uma única maneira para ser reconhecida e respeitada.

## Para que serve a Jornada de Bia?

A Jornada de Bia é desenvolvida como uma ferramenta de apoio à **comunicação alternativa e aumentativa (CAA)** e à **tecnologia assistiva**, com foco em acessibilidade e inclusão. O projeto busca facilitar o uso de recursos visuais de comunicação e oferecer uma experiência que possa ser adaptada às preferências de quem utiliza o aplicativo.

O projeto pode interessar a pessoas autistas, familiares, cuidadores, educadores e profissionais que acompanham pessoas com diferentes necessidades de comunicação. A adequação a cada pessoa deve ser avaliada no contexto de uso; o aplicativo não substitui orientação individualizada quando ela for necessária.

## Recursos do projeto

- **Cartões e símbolos de comunicação:** cartões rápidos e símbolos personalizados podem ser usados para compor frases na interface.
- **Personalização visual:** temas azul, rosa e verde pastel, com opções de modo escuro.
- **Aplicação web progressiva (PWA):** configuração de instalação e cache para apoiar o uso em uma origem web compatível.
- **Armazenamento local:** camada centralizada de persistência para os dados da aplicação.
- **PIN do Modo Responsável:** derivação do PIN com PBKDF2-SHA-256 e bloqueio após tentativas malsucedidas.
- **Backups:** backup completo, backup de configuração e cópia de segurança antes da importação, com validações de estrutura e integridade.
- **Random Forest experimental:** classificador local em desenvolvimento, utilizado apenas quando há dados locais suficientes e com fallback para a lógica de previsão anterior.
- **Biometria experimental:** prova de conceito baseada em WebAuthn no navegador, sujeita à compatibilidade do dispositivo e às limitações de validação local descritas abaixo.

Os recursos podem variar conforme a versão do repositório. Consulte os changelogs para detalhes das alterações e correções de cada versão.

## Acessibilidade, autonomia e inclusão

A Jornada de Bia é construída com a intenção de apoiar formas diversas de comunicação. Recursos visuais podem ajudar algumas pessoas a indicar escolhas e necessidades, mas as preferências, habilidades e formas de comunicação variam de pessoa para pessoa.

O objetivo não é substituir a voz de ninguém nem exigir que todas as pessoas se comuniquem da mesma forma. É ampliar as possibilidades de expressão e contribuir para um cotidiano mais acessível e inclusivo.

## Segurança e privacidade

A aplicação é executada principalmente no navegador e utiliza armazenamento local. Isso significa que a segurança depende também do dispositivo, do navegador e da integridade dos arquivos da aplicação.

### Proteções implementadas no código

- O PIN legado em texto puro pode ser migrado para uma derivação PBKDF2-SHA-256 durante a verificação.
- A sessão desbloqueada não deve ser persistida em `localStorage`.
- Há bloqueio por tentativas malsucedidas.
- Os backups passam por validações de tamanho, estrutura, versão e integridade SHA-256.

### Limitações importantes

**Esses mecanismos não equivalem a uma autenticação forte validada por um servidor confiável.** Uma pessoa que controle o dispositivo, o navegador ou os arquivos da aplicação pode alterar o código ou o armazenamento local.

A biometria via WebAuthn é uma prova de conceito local nesta versão: a validação confiável de uma resposta WebAuthn por um servidor não está implementada. A aplicação não lê nem envia os dados biométricos em si, mas isso não torna o fluxo local resistente à adulteração do cliente.

Não utilize a aplicação, em sua configuração atual, como única barreira de proteção para dados médicos ou outras informações altamente sensíveis. Para esse cenário, seria necessário projetar e validar um cofre criptografado, gerenciamento de chaves e uma política explícita de recuperação.

## Random Forest: recurso experimental

O módulo `js/random-forest.js` contém um classificador experimental que funciona localmente. O modelo só participa das previsões quando há dados locais suficientes; caso contrário, a aplicação mantém a lógica de previsão anterior como alternativa.

A confiança exibida corresponde à proporção de votos das árvores e **não é uma probabilidade calibrada**. Testes com dados sintéticos ajudam a verificar o funcionamento técnico, mas não demonstram desempenho real com os dados de uso. O modelo não deve ser tratado como ferramenta clínica ou como garantia de acerto.

## Instalação e execução

A Jornada de Bia é uma aplicação web. Para testar os recursos de PWA e APIs que exigem origem segura:

1. Publique o projeto em um servidor web com HTTPS, por exemplo, GitHub Pages ou outro serviço compatível.
2. Abra o endereço publicado em um navegador atualizado.
3. Para desenvolvimento local, utilize `localhost` por meio de um servidor de desenvolvimento.
4. Não abra diretamente o arquivo `index.html` usando `file://`: Service Workers e algumas APIs do navegador exigem uma origem web válida.

A disponibilidade de instalação, cache offline e WebAuthn varia conforme o navegador, o sistema operacional e as configurações do dispositivo.

## Estrutura do projeto

```text
.
├── index.html             # Estrutura da interface
├── css/
│   └── style.css          # Estilos da aplicação
├── js/
│   ├── app.js             # Lógica principal da aplicação
│   ├── storage.js         # Persistência local centralizada
│   ├── security.js        # PIN, sessão e bloqueio por tentativas
│   ├── backup.js           # Importação e exportação de backups
│   └── random-forest.js   # Classificador experimental
├── manifest.json          # Configuração da PWA
└── sw.js                  # Cache, versionamento e atualização da PWA
```

A estrutura acima resume os principais arquivos descritos neste README. Outros módulos e arquivos de testes podem existir no repositório.

## Histórico de versões

### V31.2 — temas e modelo experimental
- Disponibilizado o tema Verde Pastel.
- Adicionado o classificador experimental de Random Forest, com fallback para a lógica anterior.

### V31.4 — consolidação de segurança e interface
- Mantida a limpeza seletiva do armazenamento.
- Encerrada a sessão do Modo Responsável durante a exclusão geral.
- Versionado o cache do Service Worker.
- Recomendada a validação da exclusão completa em navegador real antes do uso com dados importantes.

### V31.5 — identidade e aparência
- Atualizado o subtítulo do projeto.
- Reorganizadas as opções de cor e modo escuro.
- Adicionado tema escuro verde.
- Mantida a compatibilidade com preferências visuais anteriores.

### V31.6 — Random Forest experimental revisado
- Corrigida a leitura das variáveis de contexto.
- Implementados bootstrap ponderado e seleção aleatória de atributos.
- Incluídas métricas diagnósticas e testes com dados sintéticos.
- Documentadas as limitações da medida de confiança.

### V31.7 — biometria experimental no navegador
- Adicionada configuração de credencial WebAuthn quando suportada.
- Mantido o PIN como alternativa.
- Documentadas as limitações de uma validação realizada somente no cliente.

### V31.8 — correção das estatísticas
- Corrigido o comportamento de abertura e fechamento da seção Estatísticas.
- Consulte `CHANGELOG_V31_8.md`.

### V31.9 — cartões rápidos na tela inicial
- Adicionado atalho para criar símbolos.
- Símbolos personalizados passaram a aparecer primeiro na seção “Meus cartões”, com limite visual de 15 cartões.
- Atualizada a tela inicial após salvar, editar ou excluir símbolos.

### V31.18 — teste de compatibilidade biométrica
- Mantida a biometria como prova de conceito local para testes compatíveis com Chrome/Android.
- Reforçada a necessidade de HTTPS ou `localhost` e a limitação de não haver validação confiável por servidor.

### Jornada de Bia Sakura V1.1 — correção do nome do tema
- Corrigido o contraste do tooltip com o nome do tema em modos escuros.
- Mantido o girassol como ícone da aplicação.
- Reiterada a limitação da autenticação biométrica local.

### Sakura V1.3 — contraste das notificações
- Ajustados o contraste e a quebra de texto das notificações nos temas claros e escuros.
- Consulte `CHANGELOG_SAKURA_V1_3.md`.

> **Nota sobre o histórico:** as descrições acima resumem os registros fornecidos para este README. Consulte os arquivos de changelog do repositório para o histórico completo e confirme os nomes dos arquivos na versão publicada.

## Testes e validação

A execução sintática ou estrutural de testes não substitui testes de uso real. Antes de usar o aplicativo com informações importantes, valide em dispositivos e navegadores reais:

- criação, edição e exclusão de símbolos;
- exportação, importação e recuperação de backups;
- bloqueio e encerramento de sessão;
- preferências de tema;
- instalação e atualização da PWA;
- compatibilidade de recursos experimentais.

## Contribuições

Contribuições, relatos de problemas e sugestões são bem-vindos. Ao propor mudanças, procure preservar a acessibilidade, documentar limitações de segurança e não apresentar recursos experimentais como funcionalidades comprovadas.

## Palavras-chave

Jornada de Bia, comunicação alternativa e aumentativa, CAA, tecnologia assistiva, comunicação por símbolos, cartões de comunicação, acessibilidade digital, inclusão, autonomia na comunicação, recursos visuais de comunicação, apoio à comunicação de pessoas autistas, PWA, aplicação web acessível.

---

**Importante:** este README ajuda mecanismos de busca e visitantes a compreenderem o projeto, mas não garante posicionamento no Google. A indexação depende também de o repositório ou site estar público, de as páginas poderem ser rastreadas e de outros fatores técnicos e de qualidade.
