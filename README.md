# Casal do Tráfego Financeiro — primeira versão

PWA responsivo em português, com visual premium preto e dourado. Foi pensado para uso no iPhone, com:

- Cadastro/edição de clientes, mensalidade, dia de vencimento e WhatsApp.
- Visão mensal de receita prevista, Pix confirmado, saldo a receber e despesas.
- Registro manual de recebimentos Pix (não integra ao banco e não confirma sozinho).
- Registro de despesas.
- Atalho de cobrança via WhatsApp com mensagem pré-preenchida (você revisa e envia).
- Exportação CSV e exportação de lembretes do mês em arquivo `.ics` para importar no Calendário.
- PWA instalável e cache básico.
- Integração opcional com Firebase Authentication + Firestore para nuvem.

## Importante antes de usar com dados reais

O projeto vem em **modo local/demonstração por padrão**. Até configurar Firebase e publicar com HTTPS, os dados ficam apenas no navegador deste dispositivo e não sincronizam entre aparelhos. Não cadastre informações reais de clientes até completar a configuração da nuvem e testar o acesso.

## Configurar Firebase para sincronizar na nuvem

1. Crie um projeto em https://console.firebase.google.com/.
2. Ative Authentication > Sign-in method > Email/Password.
3. Crie o Firestore Database.
4. Em Configurações do projeto, registre um app Web e copie a configuração.
5. Copie `firebase-config.example.js` para `firebase-config.js` e preencha a configuração do projeto.
6. Publique as regras de `firestore.rules`. Elas limitam cada usuário ao seu próprio caminho `users/{uid}`.
7. Hospede o projeto em HTTPS (por exemplo, Firebase Hosting). Não abra `index.html` diretamente como arquivo local, porque módulos ES e autenticação exigem um servidor.

## Publicar pelo Firebase Hosting (opção econômica)

Instale Node.js e Firebase CLI num computador, entre na conta Firebase, rode `firebase init hosting`, escolha o diretório do app como public e configure SPA como não/sem rewrite obrigatório. Depois rode `firebase deploy --only hosting`. Se preferir, a configuração pode ser feita por um desenvolvedor; não compartilhe sua senha com ninguém.

## Instalar no iPhone

Depois de publicar em HTTPS, abra o endereço no Safari > botão Compartilhar > Adicionar à Tela de Início. O arquivo `.ics` gerado em “Exportar lembretes” pode ser aberto no iPhone para importar eventos no Calendário. O calendário precisa ser exportado/importado novamente a cada mês se você quiser gerar novos vencimentos.

## Limitações da primeira versão

- Sem sincronização bancária; todos os Pix são registrados manualmente.
- Sem envio automático de mensagens nem cobranças financeiras.
- O calendário é exportado como arquivo `.ics`; o app ainda não cria eventos no calendário silenciosamente.
- As notificações push do próprio app ainda não foram implementadas. Os lembretes iniciais usam o Calendário do iPhone.
- O status de cobrança acompanha um pagamento por cliente por mês; se um cliente pagar em parcelas, é necessário ampliar o modelo antes de usar essa rotina.
- O cálculo de receita prevista considera as mensalidades ativas no mês atual; contratos com início/fim e impostos não são modelados nesta versão.

## Segurança e manutenção

Faça backup CSV regularmente. Configure regras de segurança e Authentication antes de usar com dados reais. A configuração web do Firebase é identificadora do projeto, não substitui regras de segurança. Não inclua senhas, dados bancários ou documentos pessoais de clientes neste protótipo.
