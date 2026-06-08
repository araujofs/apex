# Requisitos

## Atores identificados
Como não vai precisar de autenticação, compartilhamento de tracks e nuvem inicialmente, não vai precisar de diferentes níveis de acesso ou coisa do tipo.

- Usuário

## Requisitos Funcionais

### Gerenciamento de VS

* **[RF-001]**: Como usuário, quero gerenciar VSs para organizá-los de acordo com a necessidade.
  * O usuário deve poder criar VSs vazios.
  * O usuário deve poder editar o nome dos VSs.
  * O usuário deve poder excluir VSs.
  * O usuário deve poder visualizar a lista de VSs cadastrados.
  * O usuário deve poder abrir um VS para visualizar e gerenciar suas faixas.

* **[RF-002]**: Como usuário, quero gerenciar pastas de VSs com apenas um nível de profundidade para organizá-los de acordo com a necessidade.
  * O usuário deve poder criar pastas.
  * O usuário deve poder renomear pastas.
  * O usuário deve poder excluir pastas.
  * O usuário deve poder adicionar VSs a uma pasta.
  * O usuário deve poder remover VSs de uma pasta.
  * O sistema deve permitir apenas um nível de profundidade, ou seja, uma pasta não deve conter outra pasta.

### Gerenciamento de faixas

* **[RF-101]**: Como usuário, quero importar arquivos em formato WAV ou MP3 como faixas de um VS já existente, para completar a estrutura musical do VS.
  * O usuário deve poder adicionar uma ou mais faixas a um VS existente.
  * Para cada faixa importada, o aplicativo deve criar uma pasta interna própria da faixa.
  * A pasta da faixa deve ser identificada por um ID interno, e não necessariamente pelo nome visível da faixa.
  * O nome visível da faixa deve ser salvo nos metadados do aplicativo.
  * O arquivo original importado deve ser copiado para a pasta interna da própria faixa.
  * Após a importação, o aplicativo deve gerar uma versão processada da faixa em formato WAV PCM.
  * A versão processada também deve ser salva na pasta interna da própria faixa.
  * A versão processada deve ser usada como fonte principal para reprodução.
  * O aplicativo deve manter uma organização de pastas compatível com a estrutura dos VSs.
  * O WAV processado deve ser padronizado com:
    * sample rate de 48 kHz;
    * canais preservados conforme o arquivo original;
    * bit depth de 16-bit.
  * O sistema deve armazenar metadados técnicos da faixa, incluindo:
    * ID da faixa;
    * ID do VS ao qual a faixa pertence;
    * nome visível da faixa;
    * caminho do arquivo original;
    * caminho do arquivo processado;
    * sample rate;
    * quantidade de canais;
    * bit depth;
    * duração;
    * quantidade total de frames.

* **[RF-102]**: Como usuário, quero adicionar arquivos em formato ZIP contendo várias faixas WAV ou MP3 a um VS, para preencher um VS vazio com as tracks descomprimidas.
  * O usuário deve poder selecionar um arquivo ZIP.
  * O aplicativo deve descomprimir o arquivo ZIP em uma área temporária ou pasta interna controlada pelo aplicativo.
  * O aplicativo deve identificar os arquivos de áudio suportados dentro do ZIP.
  * O aplicativo deve ignorar ou sinalizar arquivos com formato não suportado.
  * Cada arquivo WAV ou MP3 encontrado deve ser importado como uma faixa do VS.
  * Cada faixa importada a partir do ZIP deve receber sua própria pasta interna.
  * Cada faixa importada a partir do ZIP deve passar pelo mesmo processo de padronização definido no RF-101.
  * O sistema deve preservar, quando possível, os nomes originais dos arquivos como nomes iniciais das faixas.
  * Após a importação das faixas, o sistema deve limpar arquivos temporários que não sejam mais necessários.

* **[RF-103]**: Como usuário, quero gerenciar as faixas de um VS para ajustar sua organização musical.
  * O usuário deve poder renomear uma faixa.
  * O usuário deve poder excluir uma faixa.
  * O usuário deve poder visualizar informações técnicas da faixa, como duração, formato, sample rate, canais e bit depth.
  * Ao renomear uma faixa, o sistema deve alterar apenas o nome visível da faixa, sem depender da renomeação da pasta interna.
  * Ao excluir uma faixa, o sistema deve remover ou invalidar a pasta interna da faixa, incluindo o arquivo original, o arquivo processado e outros arquivos auxiliares relacionados, conforme a política de armazenamento definida pelo aplicativo.

* **[RF-104]**: Como usuário, quero que cada faixa mantenha seus arquivos organizados individualmente para facilitar gerenciamento, exclusão, reprocessamento e exportação.
  * Cada faixa deve possuir uma pasta própria dentro da pasta do VS.
  * A pasta da faixa deve conter, no mínimo:
    * o arquivo original importado;
    * o arquivo WAV processado.
  * A pasta da faixa pode conter arquivos auxiliares gerados pelo aplicativo, como forma de onda, análise de áudio ou metadados exportáveis.
  * O sistema deve associar a pasta da faixa ao ID interno da faixa.
  * A organização física dos arquivos não deve depender do nome visível definido pelo usuário.

### Importação e exportação de VS

* **[RF-151]**: Como usuário, quero exportar um VS para um arquivo ZIP para realizar backup ou transferir o VS para outro dispositivo.
  * O usuário deve poder selecionar um VS e solicitar sua exportação.
  * O sistema deve gerar um arquivo ZIP contendo os dados necessários para reconstruir o VS.
  * O arquivo exportado deve conter os metadados gerais do VS.
  * O arquivo exportado deve conter os metadados das faixas.
  * O arquivo exportado deve conter os arquivos de áudio necessários das faixas.
  * O sistema deve preservar a organização das faixas em pastas individuais dentro do pacote exportado.
  * O sistema deve incluir, no mínimo, os arquivos processados em WAV PCM.
  * O sistema pode incluir os arquivos originais importados, conforme a política de exportação definida pelo aplicativo.
  * O sistema deve informar ao usuário quando a exportação for concluída ou quando houver falha.

* **[RF-152]**: Como usuário, quero importar um VS exportado anteriormente para restaurar ou transferir um projeto.
  * O usuário deve poder selecionar um arquivo ZIP de VS exportado pelo aplicativo.
  * O sistema deve validar se o arquivo ZIP possui uma estrutura compatível com o formato de exportação do aplicativo.
  * O sistema deve ler os metadados gerais do VS.
  * O sistema deve ler os metadados das faixas.
  * O sistema deve recriar o VS no armazenamento interno do aplicativo.
  * O sistema deve recriar as pastas individuais das faixas.
  * O sistema deve copiar os arquivos de áudio do pacote importado para suas respectivas pastas internas.
  * O sistema deve restaurar as configurações das faixas, incluindo volume, mute, solo, pan e roteamento.
  * Caso alguma faixa esteja ausente, inválida ou incompatível, o sistema deve informar o problema ao usuário.
  * O sistema deve impedir a importação de pacotes corrompidos ou incompatíveis.

* **[RF-153]**: Como usuário, quero que os metadados exportados permitam reconstruir corretamente um VS em outro dispositivo.
  * O pacote exportado deve conter metadados do VS, como:
    * ID ou identificador exportável;
    * nome do VS;
    * ordem das faixas;
    * duração total;
    * data de exportação;
    * versão do formato de exportação.
  * O pacote exportado deve conter metadados de cada faixa, como:
    * ID da faixa;
    * nome visível da faixa;
    * nome do arquivo original, quando exportado;
    * nome do arquivo processado;
    * sample rate;
    * quantidade de canais;
    * bit depth;
    * duração;
    * quantidade total de frames;
    * volume;
    * mute;
    * solo;
    * pan;
    * roteamento.
  * Os metadados exportados devem ser organizados em formato estruturado, como JSON.
  * O formato dos metadados deve permitir evolução futura por meio de controle de versão.
  * O pacote exportado deve conter metadados do VS, como:
    * ID ou identificador exportável;
    * nome do VS;
    * ordem das faixas;
    * duração total;
    * BPM original, quando informado;
    * tom original, quando informado;
    * ajuste de BPM geral;
    * ajuste de tom geral;
    * data de exportação;
    * versão do formato de exportação.
  * O pacote exportado deve conter metadados de cada faixa, como:
    * ID da faixa;
    * nome visível da faixa;
    * nome do arquivo original, quando exportado;
    * nome do arquivo processado;
    * sample rate;
    * quantidade de canais;
    * bit depth;
    * duração;
    * quantidade total de frames;
    * volume;
    * mute;
    * solo;
    * pan;
    * roteamento;
    * ajuste de tom individual;
    * ajuste de BPM individual.


### Mixagem e roteamento de faixas

* **[RF-201]**: Como usuário, quero controlar o volume individual de cada faixa para ajustar o equilíbrio sonoro do VS.
  * O usuário deve poder aumentar ou diminuir o volume de cada faixa.
  * O volume configurado deve ser aplicado durante a reprodução.
  * O volume deve ser salvo junto às configurações da faixa.

* **[RF-202]**: Como usuário, quero silenciar faixas individualmente para controlar quais elementos do VS serão reproduzidos.
  * O usuário deve poder ativar ou desativar o mute de cada faixa.
  * Faixas em mute não devem contribuir para o áudio final reproduzido.
  * O estado de mute deve ser salvo junto às configurações da faixa.

* **[RF-203]**: Como usuário, quero usar solo em uma ou mais faixas para ouvir apenas faixas específicas durante a reprodução.
  * O usuário deve poder ativar ou desativar o solo de cada faixa.
  * Quando uma ou mais faixas estiverem em solo, apenas as faixas em solo devem ser reproduzidas.
  * O estado de solo deve ser salvo junto às configurações da faixa.

* **[RF-204]**: Como usuário, quero configurar o roteamento de saída de cada faixa para definir onde cada áudio será reproduzido.
  * O usuário deve poder escolher o roteamento de saída de cada faixa.
  * Inicialmente, o aplicativo deve suportar as opções:
    * Left;
    * Right;
    * Center;
    * Stereo.
  * A opção Left deve enviar a faixa apenas para o canal esquerdo.
  * A opção Right deve enviar a faixa apenas para o canal direito.
  * A opção Center deve enviar a faixa igualmente para os canais esquerdo e direito.
  * A opção Stereo deve preservar os canais esquerdo e direito originais da faixa, quando a faixa for estéreo.
  * Para faixas mono, a opção Stereo pode se comportar como Center.
  * O roteamento deve ser aplicado no mixer durante a reprodução, sem alterar necessariamente o arquivo WAV processado.

* **[RF-205]**: Como usuário, quero configurar o pan de uma faixa para posicioná-la entre os canais esquerdo e direito.
  * O usuário deve poder ajustar o pan de cada faixa.
  * O pan deve permitir posicionar a faixa à esquerda, ao centro ou à direita.
  * O pan deve ser aplicado durante a mixagem do áudio.
  * O pan deve ser salvo junto às configurações da faixa.

* **[RF-206]**: Como usuário, quero alterar o tom de uma faixa para adaptar a reprodução à tonalidade desejada.

  * O usuário deve poder ajustar o tom de uma faixa individualmente.
  * O ajuste de tom deve permitir aumentar ou diminuir a tonalidade em semitons.
  * O ajuste de tom deve ser aplicado durante a reprodução.
  * O ajuste de tom deve ser salvo junto às configurações da faixa.
  * O ajuste de tom não deve alterar necessariamente o arquivo WAV processado original da faixa.

* **[RF-207]**: Como usuário, quero alterar o BPM de uma faixa para adaptar a velocidade da reprodução à necessidade musical.

  * O usuário deve poder ajustar o BPM ou a velocidade de reprodução de uma faixa individualmente.
  * O ajuste de BPM deve permitir aumentar ou diminuir a velocidade da faixa.
  * O ajuste de BPM deve ser aplicado durante a reprodução.
  * O ajuste de BPM deve ser salvo junto às configurações da faixa.
  * O ajuste de BPM não deve alterar necessariamente o arquivo WAV processado original da faixa.

### Reprodução de VS

* **[RF-301]**: Como usuário, quero reproduzir um VS para executar todas as suas faixas de forma sincronizada.
  * O usuário deve poder iniciar a reprodução de um VS.
  * O usuário deve poder pausar a reprodução de um VS.
  * O usuário deve poder parar a reprodução de um VS.
  * Todas as faixas ativas devem iniciar a reprodução de forma sincronizada.
  * A reprodução deve usar as versões processadas das faixas em WAV PCM.
  * O aplicativo deve usar um mixer único para combinar as faixas antes de enviar o áudio para a saída.
  * O aplicativo não deve depender da execução de múltiplos players independentes para reproduzir as faixas simultaneamente.

* **[RF-302]**: Como usuário, quero que as faixas do VS sejam reproduzidas com sincronia precisa para evitar atrasos entre click, guia e tracks.
  * O sistema deve alinhar as faixas a partir de uma mesma posição inicial.
  * O sistema deve usar a mesma base de tempo para todas as faixas durante a reprodução.
  * As faixas devem ser processadas em chunks ou buffers de áudio sincronizados.
  * O mixer deve avançar todas as faixas usando a mesma contagem de frames.
  * O sistema deve evitar que uma faixa avance ou atrase em relação às demais durante a reprodução.

* **[RF-303]**: Como usuário, quero visualizar o estado da reprodução para acompanhar o andamento do VS.
  * O sistema deve exibir se o VS está parado, reproduzindo ou pausado.
  * O sistema deve exibir o tempo atual de reprodução.
  * O sistema deve exibir a duração total do VS.
  * O sistema deve permitir acompanhar o progresso da reprodução por meio de uma barra de progresso ou indicador equivalente.

* **[RF-304]**: Como usuário, quero navegar pela posição da música para iniciar a reprodução a partir de um ponto específico do VS.
  * O usuário deve poder avançar ou retroceder na reprodução.
  * O usuário deve poder selecionar uma posição específica na linha do tempo do VS.
  * Ao mudar a posição da reprodução, todas as faixas devem ser reposicionadas para o mesmo ponto temporal.
  * A nova posição deve respeitar a duração máxima do VS.

* **[RF-305]**: Como usuário, quero que o VS respeite as configurações individuais das faixas durante a reprodução.
  * O sistema deve aplicar volume individual por faixa.
  * O sistema deve aplicar mute por faixa.
  * O sistema deve aplicar solo por faixa.
  * O sistema deve aplicar pan por faixa.
  * O sistema deve aplicar o roteamento de saída configurado para cada faixa.

* **[RF-306]**: Como usuário, quero reproduzir VSs em modo split para separar click/guia e playback entre os canais esquerdo e direito.
  * O usuário deve poder configurar faixas como click, guia ou playback.
  * O usuário deve poder usar um preset de roteamento em que click e guia sejam enviados para um canal e o playback para outro.
  * O sistema deve permitir ajustar manualmente o roteamento após aplicar o preset.
  * O modo split deve ser aplicado durante a mixagem do áudio.

* **[RF-307]**: Como usuário, quero que o aplicativo valide as faixas antes da reprodução para evitar falhas durante o uso.
  * O sistema deve verificar se os arquivos processados das faixas existem.
  * O sistema deve verificar se as faixas processadas estão em formato compatível com a engine de reprodução.
  * O sistema deve verificar se as faixas processadas possuem sample rate de 48 kHz.
  * O sistema deve verificar se as faixas processadas possuem bit depth de 16-bit.
  * Caso alguma faixa esteja inválida, o sistema deve impedir a reprodução ou solicitar o reprocessamento da faixa.

* **[RF-308]**: Como usuário, quero que o aplicativo consiga reproduzir VSs mesmo com faixas mono e estéreo misturadas.
  * O sistema deve aceitar faixas mono e estéreo no mesmo VS.
  * O mixer deve converter a saída das faixas para o formato final de reprodução.
  * Faixas mono devem poder ser enviadas para Left, Right ou Center.
  * Faixas estéreo devem poder preservar seus canais originais quando configuradas como Stereo.
  * A saída final inicial do aplicativo deve ser estéreo.

* **[RF-309]**: Como usuário, quero que as configurações de reprodução de um VS sejam salvas para reutilizá-las posteriormente.
  * O sistema deve salvar volume, mute, solo, pan e roteamento de cada faixa.
  * Ao abrir novamente um VS, o sistema deve restaurar as configurações anteriores.
  * As configurações devem ser associadas ao VS e às suas respectivas faixas.

* **[RF-310]**: Como usuário, quero alterar o tom geral de um VS para adaptar todas as faixas à tonalidade desejada.

  * O usuário deve poder aumentar ou diminuir o tom geral do VS em semitons.
  * A alteração de tom geral deve ser aplicada a todas as faixas ativas durante a reprodução.
  * O sistema deve preservar a sincronia entre as faixas ao aplicar alteração de tom.
  * A configuração de tom geral deve ser salva junto às configurações do VS.
  * A alteração de tom geral não deve alterar necessariamente os arquivos WAV processados das faixas.

* **[RF-311]**: Como usuário, quero alterar o BPM geral de um VS para adaptar a velocidade da música à necessidade da execução.

  * O usuário deve poder aumentar ou diminuir o BPM geral do VS.
  * A alteração de BPM geral deve ser aplicada a todas as faixas ativas durante a reprodução.
  * O sistema deve preservar a sincronia entre as faixas ao aplicar alteração de BPM.
  * A configuração de BPM geral deve ser salva junto às configurações do VS.
  * A alteração de BPM geral não deve alterar necessariamente os arquivos WAV processados das faixas.

* **[RF-312]**: Como usuário, quero que as configurações de tom e BPM sejam respeitadas durante a reprodução do VS.

  * O sistema deve aplicar o tom geral do VS durante a reprodução.
  * O sistema deve aplicar o BPM geral do VS durante a reprodução.
  * O sistema deve aplicar ajustes individuais de tom e BPM das faixas, quando existirem.
  * O sistema deve definir uma regra de prioridade entre ajustes gerais do VS e ajustes individuais das faixas.
  * O sistema deve manter todas as faixas sincronizadas mesmo após alterações de tom ou BPM.

## Requisitos Não Funcionais

### Armazenamento local

* **[RNF-001]**: O aplicativo deve armazenar os metadados dos VSs, pastas, faixas e configurações de reprodução em um banco de dados local.

* **[RNF-002]**: O aplicativo deve utilizar SQLite, preferencialmente por meio da biblioteca Room, para persistência dos dados estruturados.

* **[RNF-003]**: O aplicativo não deve armazenar arquivos de áudio diretamente no banco de dados.

* **[RNF-004]**: Os arquivos de áudio originais e processados devem ser armazenados no sistema de arquivos interno do aplicativo.

* **[RNF-005]**: O banco de dados deve armazenar apenas referências/caminhos para os arquivos de áudio, além dos metadados técnicos e configurações das faixas.

### Organização dos arquivos

* **[RNF-006]**: Cada VS deve possuir uma estrutura própria de armazenamento interno.

* **[RNF-007]**: Cada faixa deve possuir uma pasta própria dentro da estrutura do VS.

* **[RNF-008]**: A pasta de cada faixa deve armazenar, no mínimo, o arquivo original importado e o arquivo WAV processado.

* **[RNF-009]**: A organização física dos arquivos deve usar identificadores internos, e não depender diretamente dos nomes visíveis definidos pelo usuário.

### Preferências do aplicativo

* **[RNF-010]**: O aplicativo deve armazenar preferências globais simples por meio de DataStore ou mecanismo equivalente.

* **[RNF-011]**: Preferências como último VS aberto, configurações padrão de importação, volume master padrão e tema do aplicativo podem ser armazenadas como preferências globais.

### Exportação e importação

* **[RNF-012]**: O aplicativo deve utilizar arquivos JSON ou formato estruturado equivalente para representar metadados em pacotes de exportação/importação.

* **[RNF-013]**: Os pacotes exportados devem conter uma versão do formato de exportação para permitir compatibilidade com versões futuras do aplicativo.

* **[RNF-014]**: A importação de pacotes deve validar a estrutura dos arquivos antes de recriar o VS no aplicativo.

### Reprodução e desempenho

* **[RNF-015]**: A reprodução das faixas deve priorizar sincronia entre tracks.

* **[RNF-016]**: A reprodução multitrack deve utilizar as versões processadas em WAV PCM, evitando decodificação pesada durante o playback.

* **[RNF-017]**: O aplicativo deve evitar o uso de múltiplos players independentes para reproduzir faixas simultaneamente.

* **[RNF-018]**: O processamento de áudio deve buscar reduzir travamentos, atrasos e variações de latência durante a reprodução.

### Compatibilidade de áudio

* **[RNF-019]**: O formato processado padrão das faixas deve ser WAV PCM com sample rate de 48 kHz e bit depth de 16-bit.

* **[RNF-020]**: O aplicativo deve preservar a quantidade de canais do arquivo original sempre que possível.

* **[RNF-021]**: O aplicativo deve validar os arquivos processados antes da reprodução para garantir compatibilidade com a engine de áudio.

### Processamento de áudio

* **[RNF-022]**: O aplicativo deve preservar a sincronia entre as faixas ao aplicar alterações de BPM.

* **[RNF-023]**: O aplicativo deve preservar a sincronia entre as faixas ao aplicar alterações de tom.

* **[RNF-024]**: O aplicativo deve evitar alterar permanentemente os arquivos WAV processados ao aplicar mudanças de tom ou BPM durante a reprodução.

* **[RNF-025]**: O processamento de alteração de tom e BPM deve buscar minimizar artefatos audíveis, como distorções, cortes ou perda excessiva de qualidade.

* **[RNF-026]**: As configurações de tom e BPM devem ser persistidas no banco de dados local junto às configurações do VS ou das faixas.

* **[RNF-027]**: As configurações de tom e BPM devem ser incluídas nos metadados de exportação/importação do VS.

--- 

### Mixagem e roteamento de faixas


---

### Reprodução de VS


---

### Importação e exportação de VS

Adicionar ao **[RF-153]**:


---

## Requisitos Não Funcionais


