# Publicação Android — Checklist App

## Projeto existente
- PWA: https://checklist-app-kamuytora-5569.vercel.app
- Manifesto: https://checklist-app-kamuytora-5569.vercel.app/manifest.json
- Nome de exibição: Checklist App
- Versão inicial proposta: 1.0.0 (versionCode 1)
- ID de pacote proposto (confirmar antes da criação): com.kamuytora.checklistapp
- Distribuição: Google Play Store / Android
- Arquitetura proposta: Trusted Web Activity (TWA) usando Bubblewrap

## Preparação do projeto Android
No computador com Node.js, Java e Android SDK disponíveis:

```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest https://checklist-app-kamuytora-5569.vercel.app/manifest.json
bubblewrap build
```

No assistente de inicialização:
1. Confirmar o nome e o identificador do pacote (o ID é permanente na Play Store).
2. Configurar o target SDK para API 36 ou superior e verificar as dependências Android.
3. Gerar e guardar a chave de assinatura com segurança; não adicionar a chave ou senhas ao GitHub.
4. Gerar o arquivo `app-release-bundle.aab` e testar o APK em aparelho Android.
5. Obter no Google Play Console a impressão digital SHA-256 do certificado de assinatura do aplicativo (não confundir com a chave de upload).
6. Criar `/.well-known/assetlinks.json` com o pacote e a impressão digital real e confirmar HTTP 200 com JSON.
7. Testar que a TWA abre sem barra do navegador, que login, tarefas, calendário e links funcionam.

## Asset Links — modelo, NÃO PUBLICAR com valores fictícios
```json
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.kamuytora.checklistapp",
    "sha256_cert_fingerprints": ["SUBSTITUIR_PELA_IMPRESSAO_DIGITAL_REAL_DO_GOOGLE_PLAY"]
  }
}]
```

## Preparação para a loja
- Criar/validar conta Play Console e preencher dados do desenvolvedor.
- Confirmar nome definitivo, contato de suporte, política de privacidade publicada e exclusão de conta.
- Preencher declaração de segurança dos dados com base nas práticas reais do Supabase e dos serviços utilizados.
- Preparar ícone PNG 512×512, feature graphic 1024×500 e capturas reais do app Android.
- Testar autenticação, criação/edição/exclusão de tarefas, uso offline e comportamento das notificações.
- Testar em dispositivo Android e corrigir problemas de instalação e navegação.
- Para contas pessoais novas abrangidas pela regra, teste fechado com pelo menos 12 pessoas por 14 dias antes do acesso à produção.

## Limitações atuais
- O manifesto atual tem ícone SVG, mas ainda faltam ícones PNG para compatibilidade mais ampla.
- Lembretes locais funcionam somente enquanto o aplicativo está aberto; não anunciar push em segundo plano.
- Não foi gerado AAB nem criada conta Google Play neste passo.
- Não publicar uma política de privacidade genérica sem verificar dados coletados, retenção e exclusão.

## Referências
- https://developer.android.com/develop/ui/views/layout/webapps/guide-trusted-web-activities-version2
- https://github.com/GoogleChromeLabs/bubblewrap/tree/main/packages/cli
- https://support.google.com/googleplay/android-developer/answer/11926878
- https://support.google.com/googleplay/android-developer/answer/14151465
