# Essencial Click

Site em React/Vite com painel de conteúdo, autenticação, banco de dados e armazenamento de mídia no Supabase. Os orçamentos são salvos no Supabase e preparados para envio pelo WhatsApp do estúdio.

## Configuração

1. Instale as dependências com `npm install`.
2. Crie um projeto Supabase e copie `.env.example` para `.env.local`, preenchendo a URL e a chave pública anon.
3. Execute `supabase/migrations/202609290001_site_cms.sql` no SQL Editor do projeto Supabase.
4. Em Authentication, crie o usuário do proprietário. Copie o UUID do usuário e cadastre-o como administrador:

```sql
insert into public.site_admins (user_id) values ('UUID-DO-USUARIO');
```

5. Instale/autentique o Supabase CLI, vincule o projeto e publique a função sem exigir login do visitante (o formulário é público e aplica validação e limite de envios no servidor):

```bash
npx supabase login
npx supabase link --project-ref SEU_PROJECT_REF
npx supabase functions deploy submit-quote --no-verify-jwt
```

O usuário clica em **Continuar no WhatsApp** e confirma o envio no próprio WhatsApp. O pedido também fica salvo no painel, mesmo que o cliente não conclua essa etapa. Chaves de serviço do Supabase nunca devem ser adicionadas ao frontend.

## Desenvolvimento

```bash
npm run dev
```

O painel fica em `/admin/login`. O proprietário define o WhatsApp de contato e o e-mail público em **Contato e orçamento**. Fotos podem ser enviadas ao bucket público `site-media`; vídeos podem ser enviados em MP4/WebM de até 50 MB ou incorporados via YouTube/Instagram.

## Publicação

O Supabase não hospeda o frontend Vite deste projeto. Use a Vercel para o site; o Supabase continua hospedando autenticação, banco, fotos/vídeos e a Edge Function.

1. Na pasta do projeto, execute `npx vercel` e siga as instruções para entrar/criar sua conta e criar um projeto. A Vercel detecta Vite e usa `npm run build` com saída em `dist`.
2. No dashboard da Vercel, abra **Project Settings → Environment Variables** e cadastre `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY`, com os valores públicos já usados em `.env.local`. Marque os ambientes Production e Preview.
3. Publique a versão de produção no terminal com `npx vercel --prod`.
4. Teste as rotas `/`, `/casamento`, `/orcamento` e `/admin/login` no endereço `.vercel.app` criado pela Vercel.

O arquivo `vercel.json` mantém as rotas do React Router funcionando quando acessadas diretamente. Não cadastre `SUPABASE_SECRET_KEY`, PAT ou qualquer chave de serviço nas variáveis `VITE_*`.

Depois, se quiser usar um domínio próprio, adicione-o nas configurações do projeto Vercel e copie os registros DNS que a Vercel fornecer para o Registro.br. A hospedagem do frontend não guarda uploads: o Supabase Storage e a Edge Function continuam como backend.
