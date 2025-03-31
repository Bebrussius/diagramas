```mermaid

C4Context
  title Diagrama de Contexto C4 para Sistema de Educação Baseado em Microsserviços
  Enterprise_Boundary(b0, "Limite da Plataforma") {
    Person(aluno, "Aluno", "Usuário que acessa conteúdos educacionais, realiza quizzes e acompanha seu progresso.")
    Person(instrutor, "Instrutor", "Usuário que cria e gerencia conteúdos educacionais.")
    Person(admin, "Administrador", "Gerencia a plataforma, relatórios e monitoramento.")

    System(plataforma_educacao, "Plataforma de Educação", "Sistema baseado em microsserviços para educação online.")

    Enterprise_Boundary(b1, "Backend Services") {
      SystemDb_Ext(bd_relacional, "Banco de Dados Relacional", "Armazena dados estruturados como usuários, cursos e avaliações.")
      SystemDb_Ext(bd_nosql, "Banco de Dados NoSQL", "Armazena dados não estruturados ou semi-estruturados.")
      
      System_Ext(armazenamento_arquivos, "Armazenamento de Arquivos", "Armazena vídeos, PDFs e materiais de aula.")
      System_Ext(gateway_api, "API Gateway", "Roteia requisições e gerencia segurança.")
      System_Ext(servico_cache, "Serviço de Cache", "Melhora desempenho com cache de dados.")
      System_Ext(fila_mensagens, "Fila de Mensagens", "Processa tarefas assíncronas.")
      
      System_Boundary(b2, "Microsserviços") {
        System(servico_autenticacao, "Autenticação", "Gerencia registro, login e autorização.")
        System(servico_conteudo, "Gestão de Conteúdo", "Gerencia cursos e materiais educacionais.")
        System(servico_aulas, "Aulas", "Controla acesso aos materiais de aula.")
        System(servico_avaliacao, "Avaliações", "Gerencia quizzes, notas e feedback.")
        System(servico_admin, "Administração", "Provê relatórios e monitoramento.")
      }
    }
  }

  BiRel(aluno, plataforma_educacao, "Acessa")
  BiRel(instrutor, plataforma_educacao, "Publica conteúdo")
  BiRel(admin, plataforma_educacao, "Administra")
  
  Rel(plataforma_educacao, gateway_api, "Envia requisições", "HTTPS")
  Rel(gateway_api, servico_autenticacao, "Roteia para")
  Rel(gateway_api, servico_conteudo, "Roteia para")
  Rel(gateway_api, servico_aulas, "Roteia para")
  Rel(gateway_api, servico_avaliacao, "Roteia para")
  Rel(gateway_api, servico_admin, "Roteia para")
  
  Rel(servico_autenticacao, bd_relacional, "Ler/Escrever")
  Rel(servico_conteudo, bd_relacional, "Ler/Escrever")
  Rel(servico_aulas, armazenamento_arquivos, "Acessa arquivos")
  Rel(servico_avaliacao, bd_relacional, "Ler/Escrever")
  Rel(servico_admin, bd_nosql, "Ler/Escrever")
  
  Rel(servico_conteudo, servico_cache, "Utiliza cache")
  Rel(servico_aulas, fila_mensagens, "Envia tarefas")

  UpdateElementStyle(aluno, $fontColor="green", $bgColor="white")
  UpdateElementStyle(instrutor, $fontColor="blue", $bgColor="white")
  UpdateElementStyle(admin, $fontColor="red", $bgColor="white")
  
  UpdateRelStyle(aluno, plataforma_educacao, $textColor="green", $lineColor="green")
  UpdateRelStyle(instrutor, plataforma_educacao, $textColor="blue", $lineColor="blue")
  UpdateRelStyle(admin, plataforma_educacao, $textColor="red", $lineColor="red")
  
  UpdateRelStyle(plataforma_educacao, gateway_api, $textColor="purple", $lineColor="purple")
  
  UpdateRelStyle(gateway_api, servico_autenticacao, $textColor="#0066cc", $lineColor="#0066cc")
  UpdateRelStyle(gateway_api, servico_conteudo, $textColor="#0066cc", $lineColor="#0066cc")
  UpdateRelStyle(gateway_api, servico_aulas, $textColor="#0066cc", $lineColor="#0066cc")
  UpdateRelStyle(gateway_api, servico_avaliacao, $textColor="#0066cc", $lineColor="#0066cc")
  UpdateRelStyle(gateway_api, servico_admin, $textColor="#0066cc", $lineColor="#0066cc")
  
  UpdateRelStyle(servico_autenticacao, bd_relacional, $textColor="orange", $lineColor="orange")
  UpdateRelStyle(servico_conteudo, bd_relacional, $textColor="orange", $lineColor="orange")
  UpdateRelStyle(servico_avaliacao, bd_relacional, $textColor="orange", $lineColor="orange")
  UpdateRelStyle(servico_admin, bd_nosql, $textColor="orange", $lineColor="orange")
  
  UpdateRelStyle(servico_aulas, armazenamento_arquivos, $textColor="#009900", $lineColor="#009900")
  UpdateRelStyle(servico_conteudo, servico_cache, $textColor="#990099", $lineColor="#990099")
  UpdateRelStyle(servico_aulas, fila_mensagens, $textColor="#cc0000", $lineColor="#cc0000")

  UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="2")