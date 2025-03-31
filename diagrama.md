```mermaid

graph TD
    %% Usuários do sistema
    subgraph Usuários
        usuario_estudante[Usuário Estudante]
        usuario_professor[Usuário Professor]
        usuario_admin[Administrador]
    end

    %% Camada de Entrada
    usuario_estudante -->|Acessa| sistema(Sistema Educacional Online)
    usuario_professor -->|Acessa| sistema
    usuario_admin -->|Gerencia| sistema
    sistema -->|Roteia requisições| api[API Gateway]

    %% Serviços principais agrupados
    subgraph Microsserviços
        servico_auth_autorizacao[Autenticação & Autorização]
        servico_gerenciamento[Gerenciamento de Conteúdo]
        servico_aula[Aulas & Conteúdos]
        servico_avaliacao[Avaliação & Feedback]
        servico_administracao[Administração]
    end
    api --> Microsserviços

    %% Armazenamento de Dados e Cache
    subgraph Armazenamento
        banco_relacional[(PostgreSQL/MySQL)]
        cache[Redis]
        Armazenamento_AWS[AWS S3]
    end
    Microsserviços -->|Armazena e consulta dados| banco_relacional
    Microsserviços -->|Utiliza cache| cache
    Microsserviços -->|Armazena materiais| Armazenamento_AWS

    %% Processamento Assíncrono e Monitoramento
    subgraph Infraestrutura
        fila[AWS SQS]
        monitoramento[Prometheus & AWS CloudWatch]
    end
    Microsserviços -->|Mensagens assíncronas| fila
    Microsserviços -->|Monitoramento e logs| monitoramento
