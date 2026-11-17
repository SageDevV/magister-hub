# Funcionalidade de Exclusão de Atividades

## Resumo
Adicionada uma nova funcionalidade que permite ao professor **excluir permanentemente** atividades, além da opção existente de arquivá-las.

## Mudanças Implementadas

### 1. Backend - main.js
- **Nova função `deleteActivityPermanently(activityId)`**
  - Localização: após a função `archiveActivityOnFreeTier()`
  - Verifica se o usuário é admin (professor)
  - Remove a atividade do banco de dados
  - Também remove todas as submissões relacionadas à atividade usando batch operations
  - Garante exclusão permanente e irreversível

- **Nova função window `window.deleteActivity(activityId)`**
  - Pede confirmação dupla do usuário para evitar exclusões acidentais
  - Primeira confirmação: "Excluir esta atividade permanentemente? Todas as entregas também serão removidas. Esta ação não pode ser desfeita."
  - Segunda confirmação: "Tem certeza? Esta é uma ação irreversível."
  - Atualiza mensagem de feedback ao usuário
  - Recarrega a lista de atividades após conclusão

### 2. UI - main.js (renderTeacherActivityActions)
- Adicionado novo botão "Excluir" com classe CSS `delete-exam-btn danger-btn`
- Botão aparece ao lado do botão "Arquivar" nas ações de atividades
- Apenas visível para o professor (admin)
- Desabilitado para atividades já arquivadas

### 3. Estilos - style.css
- Adicionada nova classe `.delete-exam-btn.danger-btn`
- Cor mais intensa (vermelho vivo #ff6b6b) para indicar ação perigosa
- Estilo diferenciado do botão de arquivo para alertar sobre a irreversibilidade
- Efeitos hover mais evidentes para indicar perigo

### 4. Regras de Segurança - firestore.rules
- **Atividades (activities)**
  - Alterado de `allow delete: if false;` para `allow delete: if isAdmin();`
  - Apenas o professor pode deletar atividades

- **Submissões (activitySubmissions)**
  - Alterado de `allow delete: if false;` para `allow delete: if isAdmin();`
  - Permite que o professor delete submissões ao excluir uma atividade

## Diferenças: Arquivar vs. Excluir

| Aspecto | Arquivar | Excluir |
|---------|----------|--------|
| **Dados** | Preserva tudo | Remove atividade e submissões |
| **Acesso** | Somente consulta | Dados removidos permanentemente |
| **Reversibilidade** | Pode reativar | Irreversível |
| **Confirmação** | 1 confirmação | 2 confirmações (dupla) |
| **Caso de uso** | Manter histórico | Limpeza permanente |

## Como Usar

### Como Professor:
1. Acesse a seção de Atividades do seu Dashboard
2. Localize a atividade que deseja excluir
3. Clique no botão **"Excluir"** (vermelho vivo)
4. Confirme a primeira mensagem
5. Confirme a segunda mensagem (confirmação dupla)
6. A atividade e todas as suas submissões serão removidas permanentemente

## Segurança

✅ **Validações implementadas:**
- Apenas o professor (admin) pode excluir atividades
- Confirmação dupla para evitar exclusões acidentais
- Mensagens claras indicando a irreversibilidade da ação
- Validação de permissão no backend

✅ **Proteção de dados:**
- Submissões são deletadas em lote (batch operation) para consistência
- Uso de Firestore batch operations para garantir atomicidade
- Regras de segurança no Firestore impedem acesso não autorizado

## Notas Técnicas

- A função `deleteActivityPermanently()` usa Firestore batch operations para garantir que atividades e submissões sejam deletadas atomicamente
- A dupla confirmação (`window.confirm()`) é uma medida adicional contra exclusões acidentais
- O botão "Excluir" não aparece para atividades já arquivadas (redundante)

## Testes Recomendados

1. ✓ Verificar se apenas o professor pode ver o botão de exclusão
2. ✓ Confirmar dupla antes de executar a exclusão
3. ✓ Validar se atividade e submissões são completamente removidas do Firestore
4. ✓ Testar se mensagens de erro aparecem corretamente
5. ✓ Verificar se a lista de atividades é atualizada após exclusão

## Próximas Melhorias Possíveis

- [ ] Adicionar soft-delete com período de recuperação
- [ ] Log de auditoria para exclusões
- [ ] Notificação por email aos alunos sobre exclusão de atividade
- [ ] Dashboard com histórico de atividades deletadas
