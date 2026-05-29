
<style>
  h2.sr-only { position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); white-space:nowrap; border:0; }
  * { box-sizing: border-box; margin:0; padding:0; }
  body { font-family: var(--font-sans); }
  #org-root { padding: 1rem 0.5rem 2rem; overflow-x: auto; }
  .tree { display: flex; flex-direction: column; align-items: center; min-width: max-content; }
  .node-wrap { display: flex; flex-direction: column; align-items: center; }
  .node-btn {
    display: inline-flex; align-items: center; justify-content: center; gap: 6px;
    text-align: center; padding: 8px 14px; border-radius: var(--border-radius-md);
    border: 0.5px solid transparent; font-family: var(--font-sans);
    font-size: 13px; font-weight: 500; line-height: 1.35; cursor: pointer;
    transition: transform .15s, box-shadow .15s, opacity .15s;
    white-space: normal; max-width: 200px; min-width: 120px;
  }
  .node-btn:hover { transform: translateY(-2px); filter: brightness(1.08); }
  .node-btn.leaf { cursor: default; }
  .node-btn.leaf:hover { transform: none; filter: none; }
  .toggle-ico { font-size: 10px; flex-shrink: 0; transition: transform .25s; display: inline-block; }
  .collapsed > .node-btn .toggle-ico { transform: rotate(-90deg); }
  .conn-v { width: 1.5px; background: #aaa; flex-shrink: 0; }
  .children-area {
    display: flex; flex-direction: column; align-items: center;
    overflow: hidden; transition: max-height .35s ease, opacity .3s ease;
    max-height: 8000px; opacity: 1;
  }
  .collapsed > .children-area { max-height: 0 !important; opacity: 0; pointer-events: none; }
  .children-row { display: flex; align-items: flex-start; }
  .child-col { display: flex; flex-direction: column; align-items: center; }
  .h-bar { height: 1.5px; background: #888; flex-shrink: 0; }

  /* Level colors */
  .lvl0 { background: #1a2744; color: #e8c96a; border-color: #c8a84b; font-size: 15px; min-width: 150px; }
  .lvl1 { background: #2a4494; color: #fff; border-color: #3a5bbf; min-width: 160px; }
  .lvl2 { background: #3a5bbf; color: #fff; border-color: #5a7dd4; min-width: 180px; }
  .lvl3 { background: #5a7dd4; color: #fff; border-color: #7a9de8; font-size: 12px; min-width: 170px; max-width: 210px; }
  .lvl4 { background: #3d7ab5; color: #fff; border-color: #5a9ad0; font-size: 12px; min-width: 165px; max-width: 210px; }
  .lvl5 { background: #6fa3cc; color: #fff; border-color: #8fc0df; font-size: 11px; min-width: 155px; max-width: 205px; font-weight: 400; }
  .lvl6 { background: #90bcd8; color: #1a2744; border-color: #b0d5ec; font-size: 11px; min-width: 148px; max-width: 200px; font-weight: 400; }
  .lvl7 { background: #b8d8ec; color: #1a2744; border-color: #cee8f8; font-size: 10px; min-width: 140px; max-width: 195px; font-weight: 400; }

  /* Header */
  .org-header { background: #1a2744; padding: 16px 24px; display: flex; align-items: center; gap: 14px; border-bottom: 3px solid #c8a84b; border-radius: var(--border-radius-lg) var(--border-radius-lg) 0 0; }
  .org-header .icon { width: 46px; height: 46px; border-radius: 50%; background: #c8a84b; display: flex; align-items: center; justify-content: center; font-size: 22px; flex-shrink: 0; }
  .org-header h1 { color: #f8f6f0; font-size: 17px; font-weight: 500; line-height: 1.2; }
  .org-header p { color: #e8c96a; font-size: 11px; letter-spacing: .08em; text-transform: uppercase; margin-top: 2px; }

  .org-intro { text-align: center; padding: 18px 16px 8px; }
  .org-intro p { font-size: 12px; color: var(--color-text-secondary); margin-top: 4px; }
  .hint { display: inline-flex; align-items: center; gap: 5px; margin-top: 8px; background: var(--color-background-secondary); border: 0.5px solid var(--color-border-secondary); border-radius: 20px; padding: 4px 12px; font-size: 11px; color: var(--color-text-secondary); }

  .legend { display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; padding: 0 16px 20px; }
  .leg-item { display: flex; align-items: center; gap: 6px; font-size: 11px; color: var(--color-text-secondary); }
  .leg-dot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; }

  @media (prefers-color-scheme: dark) {
    .lvl6, .lvl7 { color: #0a1628; }
  }

  /* Linhas de conexão contínuas */
.conn-v {
  width: 1.5px;
  background: #999;
  flex-shrink: 0;
}

.conn-h {
  height: 1.5px;
  background: #999;
  flex-shrink: 0;
}

.child-col {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.children-row {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Transições suaves para expansão e espaçamento */
.spacer {
  transition: width 0.3s ease !important;
}

.node-wrap {
  transition: margin 0.3s ease;
}

.children-area {
  transition: max-height 0.35s ease, opacity 0.3s ease, margin 0.3s ease;
}

/* Efeito de distanciamento visual */
.node-wrap:not(.collapsed) > .node-btn {
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.node-wrap.collapsed > .node-btn {
  box-shadow: none;
}

/* Estilos para controle de espaçamento dinâmico e transições suaves */
.spacer {
  transition: width 0.3s ease-in-out;
  width: 40px; /* Largura padrão quando todos os nós estão expandidos */
}

/* Quando um nó PAI (que contém .children-area) está recolhido, ele afeta o espaçador à sua DIREITA */
/* Para isso, precisamos estilizar o espaçador que está no mesmo .children-row que o nó recolhido */

/* Regra complexa: Quando um .child-col tem um .node-wrap recolhido, encontra o próximo .spacer e reduz sua largura */
.children-row:has(.child-col .node-wrap.collapsed) .spacer {
  width: 10px !important; /* Espaço mínimo quando o vizinho da esquerda está recolhido */
}

/* Também queremos reduzir o espaço se o vizinho da DIREITA estiver recolhido? 
   Por simplicidade, vamos reduzir o espaço à direita de um nó se ele mesmo estiver recolhido.
   Isso requer uma estrutura diferente. Vamos usar uma abordagem mais simples: */

/* Estilo alternativo: todos os espaçadores começam pequenos e aumentam se o nó à esquerda estiver expandido */
.spacer {
  width: 15px; /* Valor pequeno padrão */
}

/* Se o nó à esquerda do espaçador NÃO estiver recolhido (ou seja, estiver expandido), o espaçador aumenta */
.child-col:has(.node-wrap:not(.collapsed)) + .spacer {
  width: 60px; /* Espaço generoso para nós expandidos */
}

/* Isso cria o efeito desejado: nós expandidos "empurram" o próximo irmão para longe, 
   enquanto nós recolhidos deixam os irmãos mais próximos. */
</style>

<h2 class="sr-only">Organograma institucional da Prefeitura Municipal com estrutura de órgãos e secretarias expansível por clique</h2>

<div class="org-header">
  <div class="icon">🏛️</div>
  <div>
    <h1>Prefeitura Municipal</h1>
    <p>Estrutura Organizacional da Administração</p>
  </div>
</div>

<div class="org-intro">
  <p>Distribuição dos órgãos e secretarias que compõem a administração municipal</p>
  <span class="hint">💡 Clique em qualquer nó para expandir ou recolher seus subordinados</span>
</div>

<div id="org-root"></div>

<div class="legend">
  <div class="leg-item"><div class="leg-dot" style="background:#1a2744;border:1px solid #c8a84b"></div> Chefia Executiva</div>
  <div class="leg-item"><div class="leg-dot" style="background:#2a4494"></div> Vice-Chefia</div>
  <div class="leg-item"><div class="leg-dot" style="background:#3a5bbf"></div> Ramos Principais</div>
  <div class="leg-item"><div class="leg-dot" style="background:#5a7dd4"></div> Órgãos e Secretarias</div>
  <div class="leg-item"><div class="leg-dot" style="background:#3d7ab5"></div> Chefias Intermediárias</div>
  <div class="leg-item"><div class="leg-dot" style="background:#6fa3cc"></div> Coordenações</div>
  <div class="leg-item"><div class="leg-dot" style="background:#90bcd8"></div> Supervisões</div>
  <div class="leg-item"><div class="leg-dot" style="background:#b8d8ec"></div> Subníveis Operacionais</div>
</div>

<script>
const org = {
  id:'prefeito', label:'Prefeito', level:0,
  children:[{
    id:'vp', label:'Vice-Prefeito', level:1,
    children:[
      {
        id:'autonomos', label:'Órgãos Autônomos', level:2,
        children:[
          { id:'gabinete', label:'Gabinete do Prefeito', level:3,
            children:[{ id:'chefe-gab', label:'Chefe de Gabinete', level:4,
              children:[
                { id:'coord-atend', label:'Coord. I de Atendimentos aos Munícipes', level:5 },
                { id:'coord-econ',  label:'Coord. I de Desenvolvimento Econômico', level:5 },
                { id:'coord-impr',  label:'Coord. I de Comunicação e Imprensa', level:5 },
              ]
            }]
          },
          { id:'juridica', label:'Assessoria Jurídica do Município', level:3,
            children:[{ id:'assessor-jur', label:'Assessor Jurídico', level:4,
              children:[{ id:'assessor-jur-i', label:'Assessor Jurídico I', level:5 }]
            }]
          },
          { id:'projetos', label:'Setor de Projetos, Convênios e Captação', level:3,
            children:[{ id:'super-proj', label:'Superintendente de Projetos e Convênios', level:4,
              children:[
                { id:'resp-eng',      label:'Resp. Técnico de Engenharia e Projetos', level:5 },
                { id:'resp-arq',      label:'Resp. Técnico de Arquitetura e Urbanismo', level:5 },
                { id:'resp-ref',      label:'Resp. Técnico de Reformas e Ampliações', level:5 },
                { id:'coord-analise', label:'Coord. I de Análise de Projetos', level:5 },
                { id:'coord-conv',    label:'Coord. I de Convênios e Captação', level:5 },
                { id:'super-fisc',    label:'Supervisor de Fiscalização de Obras', level:5 },
              ]
            }]
          },
        ]
      },
      {
        id:'secretarias', label:'Secretarias Municipais', level:2,
        children:[
          { id:'adm', label:'Sec. de Administração e RH', level:3,
            children:[{ id:'sec-adm', label:'Secretário de Administração e RH', level:4,
              children:[
                { id:'super-rh', label:'Superintendente de RH', level:5,
                  children:[
                    { id:'coord-cadastro', label:'Coord. II de Cadastro e Protocolos', level:6 },
                    { id:'super-acesso',   label:'Supervisor de Controle de Acesso', level:6 },
                  ]
                },
                { id:'super-adm', label:'Superintendente de Administração', level:5,
                  children:[
                    { id:'coord-seg',   label:'Coord. I de Segurança e Defesa Civil', level:6 },
                    { id:'coord-ti',    label:'Coord. I de TI e Transparência', level:6 },
                    { id:'super-vigil', label:'Supervisor de Vigilância Patrimonial', level:6 },
                    { id:'super-arq',   label:'Supervisor de Arquivo Público', level:6 },
                    { id:'coord-alm',   label:'Coord. I do Almoxarifado Geral', level:6 },
                    { id:'super-alm',   label:'Supervisor de Almoxarifado e Patrimônio', level:6 },
                  ]
                },
              ]
            }]
          },
          { id:'contab', label:'Sec. de Contabilidade e Orçamento', level:3,
            children:[{ id:'sec-contab', label:'Secretário de Contabilidade e Orçamento', level:4,
              children:[{ id:'super-contab', label:'Superintendente de Contabilidade', level:5,
                children:[
                  { id:'coord-contab',   label:'Coord. II de Assuntos Contábeis', level:6 },
                  { id:'coord-dados',    label:'Coord. II de Administração de Dados', level:6 },
                  { id:'super-adm-cont', label:'Supervisor Administrativo Contábil', level:6 },
                ]
              }]
            }]
          },
          { id:'financas', label:'Sec. de Finanças e Arrecadação', level:3,
            children:[{ id:'sec-fin', label:'Secretário de Finanças e Arrecadação', level:4,
              children:[
                { id:'super-arrec', label:'Superintendente de Arrecadação', level:5,
                  children:[
                    { id:'coord-arrec',   label:'Coord. II de Arrecadação e Fiscalização', level:6 },
                    { id:'super-imob',    label:'Supervisor de Cadastro Imobiliário', level:6 },
                    { id:'super-faz',     label:'Supervisor de Serviço Fazendário', level:6 },
                    { id:'assist-arrec',  label:'Assistente de Arrecadação', level:6 },
                  ]
                },
                { id:'coord-fin',    label:'Coord. II de Finanças', level:5 },
                { id:'encarreg-fin', label:'Encarregado de Serviços Financeiros', level:5 },
              ]
            }]
          },
          { id:'compras', label:'Sec. de Compras e Licitação', level:3,
            children:[{ id:'sec-compras', label:'Secretário de Compras e Licitação', level:4,
              children:[{ id:'super-contratos', label:'Supervisor de Contratos e Entregas', level:5 }]
            }]
          },
          { id:'distrital', label:'Sec. de Administração Distrital', level:3,
            children:[{ id:'sec-dist', label:'Secretário de Administração Distrital', level:4,
              children:[
                { id:'super-dist-infra',   label:'Supervisor Distrital de Infraestrutura', level:5 },
                { id:'super-dist-serv',    label:'Supervisor Distrital de Serviços Urbanos', level:5 },
                { id:'encarreg-distrital', label:'Encarregado Distrital', level:5 },
              ]
            }]
          },
          { id:'obras', label:'Sec. de Obras e Infraestrutura', level:3,
            children:[{ id:'sec-obras', label:'Secretário de Obras e Infraestrutura', level:4,
              children:[{ id:'super-obras', label:'Superintendente de Obras', level:5,
                children:[
                  { id:'super-solda',    label:'Supervisor de Serviços de Solda', level:6 },
                  { id:'super-pintura',  label:'Supervisor de Pintura e Sinalização', level:6,
                    children:[{ id:'super-reforma', label:'Supervisor de Reforma e Ampliação', level:7 }]
                  },
                  { id:'super-concert',   label:'Supervisor de Concertos e Reparos', level:6 },
                  { id:'coord-elet',      label:'Coord. II de Serviços Elétricos e Iluminação', level:6 },
                  { id:'coord-pracas',    label:'Coord. II de Praças, Parques e Jardins', level:6 },
                  { id:'encarreg-fisc',   label:'Encarregado de Fiscalização e Postura', level:6 },
                  { id:'encarreg-dren',   label:'Encarregado de Drenagem e Esgoto', level:6 },
                  { id:'super-limpeza',   label:'Supervisor de Limpeza Pública', level:6 },
                ]
              }]
            }]
          },
          { id:'transp', label:'Sec. de Transporte e Estradas', level:3,
            children:[
              { id:'sec-transp', label:'Secretário Municipal de Transporte e Estradas', level:4,
                children:[
                  { id:'super-transp', label:'Superintendente de Transportes e Estradas', level:5,
                    children:[
                      { id:'coord-estradas', label:'Coordenador II de Serviços de Estradas', level:6 },
                      { id:'coord-mecanicos', label:'Coordenador III de Serviços Mecânicos', level:6 },
                      { id:'super-estradas', label:'Supervisor de Serviços de Estradas', level:6 },
                    ]
                  }
                ]
              }
            ]
          },
          { id:'agri', label:'Sec. de Agricultura, Pecuária e Meio Ambiente', level:3,
            children:[
              { id:'sec-agri', label:'Secretário de Agricultura, Pecuária e Meio Ambiente', level:4,
                children:[
                  { id:'super-agri', label:'Superintendente de Agricultura', level:5,
                    children:[
                      { id:'coord-agricultura', label:'Coord. I de Agricultura Familiar', level:6 },
                      { id:'coord-pecuaria', label:'Coord. I de Pecuária e Pastagens', level:6 },
                      { id:'coord-irrigacao', label:'Coord. I de Irrigação e Recursos Hídricos', level:6 },
                      { id:'super-defesa', label:'Supervisor de Defesa Agropecuária', level:6 },
                    ]
                  },
                  { id:'super-meio', label:'Superintendente de Meio Ambiente', level:5,
                    children:[
                      { id:'coord-licencas', label:'Coord. I de Licenciamento Ambiental', level:6 },
                      { id:'coord-residuos', label:'Coord. I de Resíduos Sólidos', level:6 },
                      { id:'coord-parques', label:'Coord. I de Parques e Áreas Verdes', level:6 },
                      { id:'super-fiscal', label:'Supervisor de Fiscalização Ambiental', level:6 },
                      { id:'super-educacao', label:'Supervisor de Educação Ambiental', level:6 },
                    ]
                  },
                  { id:'coord-pesca', label:'Coord. II de Pesca e Aquicultura', level:5 },
                  { id:'coord-agroindustria', label:'Coord. II de Agroindústria', level:5 },
                  { id:'encarregado-viveiro', label:'Encarregado de Viveiro Municipal', level:5 },
                ]
              }
            ]
          },
          { id:'social', label:'Sec. de Assistência Social e Habitação', level:3,
            children:[
              { id:'sec-social', label:'Secretário Municipal de Assistência Social e Habitação', level:4,
                children:[
                  { id:'coord-adm-social', label:'Coordenador I de Administração de Assistência Social e Habitação', level:5,
                    children:[
                      { id:'super-social', label:'Superintendente Municipal de Assistência Social e Habitação', level:6,
                        children:[
                          { id:'coord-programas-sociais2', label:'Coordenador II de Programas Sociais', level:7 },
                          { id:'coord-programas-sociais3', label:'Coordenador III de Programas Sociais', level:7,
                            children:[
                              { id:'super-programas-habit', label:'Supervisor de Programas Habitacionais', level:8 },
                              { id:'coord-desenv-social3', label:'Coordenador III de Desenvolvimento Social', level:8,
                                children:[
                                  { id:'super-desenv-social', label:'Supervisor de Programas de Desenvolvimento Social', level:9 },
                                ]
                              },
                            ]
                          },
                          { id:'encarregado-acolhimento', label:'Encarregado da Casa de Acolhimento', level:7 },
                          { id:'assist-servicos', label:'Assistente de Serviços em Assistência Social', level:7 },
                          { id:'assist-juridico', label:'Assistente Jurídico', level:7 },
                          { id:'assist-programas', label:'Assistente de Programas Sociais', level:7 },
                        ]
                      }
                    ]
                  }
                ]
              }
            ]
          },
          { id:'educ', label:'Sec. de Educação, Cultura e Turismo', level:3,
            children:[
              { id:'sec-educ', label:'Secretário Municipal de Educação, Cultura e Turismo', level:4,
                children:[
                  { id:'coord-educ-especial', label:'Coordenador I de Educação Especial e Assistência', level:5 },
                  { id:'coord-educ-infantil2', label:'Coordenador II de Educação Infantil', level:5 },
                  { id:'coord-educ-infantil3', label:'Coordenador III de Educação da Educação Infantil', level:5 },
                  { id:'coord-anos-iniciais', label:'Coordenador IV de Anos Iniciais do Ensino', level:5 },
                  { id:'coord-anos-finais', label:'Coordenador V de Anos Finais do Ensino', level:5 },
                  { id:'secretario-escola', label:'Secretário de Escola', level:5,
                    children:[
                      { id:'assist-supervisao', label:'Assistente de Supervisão Escolar', level:6 },
                    ]
                  },
                  { id:'diretor-escolar', label:'Diretor Escolar', level:5,
                    children:[
                      { id:'super-cultura-turismo', label:'Superintendente de Cultura e Turismo', level:6,
                        children:[
                          { id:'patrimonio-historico', label:'Patrimônio Histórico e Cultural', level:7 },
                        ]
                      },
                    ]
                  },
                  { id:'vice-diretor-iniciais', label:'Vice-Diretor de Anos Iniciais do Ensino', level:5 },
                  { id:'registros-escolares', label:'Coordenador de Registros Escolares e Sistemas', level:5 },
                  { id:'projetos-interdisciplinar', label:'Coordenador de Projetos Interdisciplinares', level:5 },
                  { id:'patrimonio-almoxarifado', label:'Coordenador de Patrimônio e Almoxarifado', level:5 },
                  { id:'adm-secretaria', label:'Administrativo da Secretaria', level:5 },
                  { id:'licitacao-contrato', label:'Coordenador de Licitação e Contratos', level:5 },
                  { id:'coord-logistica-transporte', label:'Coordenador de Logística e Transporte', level:5 },
                ]
              }
            ]
          },
          { id:'esportes', label:'Sec. de Esportes e Lazer', level:3,
            children:[
              { id:'sec-esportes', label:'Secretário Municipal de Esportes e Lazer', level:4,
                children:[
                  { id:'coord-projetos', label:'Coordenador I de Projetos Esportivos', level:5,
                    children:[
                      { id:'super-atividades', label:'Supervisor de Atividades Esportivas', level:6 },
                    ]
                  }
                ]
              }
            ]
          },
          { id:'saude', label:'Sec. de Saúde', level:3,
            children:[
              { id:'sec-saude', label:'Secretário Municipal de Saúde', level:4,
                children:[
                  { id:'super-saude', label:'Superintendente de Saúde', level:5,
                    children:[
                      { id:'resp-pam', label:'Responsável Técnico de Pronto Atendimento Médico - PAM', level:6,
                        children:[
                          { id:'coord-transporte-urgencia-pam', label:'Coordenador II de Transporte de Urgência', level:7 },
                        ]
                      },
                      { id:'coord-med-regulacao', label:'Coordenador I Médico de Regulação', level:6 },
                      { id:'coord-atencao-basica', label:'Coordenador I de Atenção Básica', level:6,
                        children:[
                          { id:'coord-saude-bucal', label:'Coordenador I de Saúde Bucal', level:7 },
                          { id:'super-atencao-saude', label:'Supervisor de Atenção em Saúde', level:7 },
                          { id:'super-unidade-saude', label:'Supervisor de Unidade de Saúde', level:7 },
                          { id:'encarregado-sistemas', label:'Encarregado de Sistemas CNES, FPO e PAB', level:7 },
                          { id:'encarregado-sisvam', label:'Encarregado de Programa SISVAM', level:7 },
                        ]
                      },
                      { id:'coord-medicamentos', label:'Coordenador II de Medicamentos, Suplementos e Insumos', level:6 },
                      { id:'coord-adm-vigilancia', label:'Coordenador II Administrativo da Vigilância em Saúde', level:6,
                        children:[
                          { id:'coord-vig-sanitaria', label:'Coordenador III de Vigilância Sanitária', level:7 },
                          { id:'coord-vig-epidemiologica', label:'Coordenador III de Vigilância Epidemiológica', level:7 },
                        ]
                      },
                      { id:'coord-consultas-exames', label:'Coordenador II de Consultas e Exames de Medicina', level:6 },
                      { id:'coord-adm-policlinica', label:'Coordenador II de Administrativo da Políclínica', level:6 },
                      { id:'coord-caps', label:'Coordenador II de Centro de Atenção Psicossocial - CAPS', level:6 },
                      { id:'coord-tfd', label:'Coordenador II de Tratamento Fora do Domicílio - TFD', level:6 },
                      { id:'super-almox-saude', label:'Supervisor do Almoxarifado de Saúde', level:6 },
                    ]
                  }
                ]
              }
            ]
          },
        ]
      }
    ]
  }]
};

function el(tag, cls, style) {
  const e = document.createElement(tag);
  if (cls) e.className = cls;
  if (style) Object.assign(e.style, style);
  return e;
}

function buildNode(node) {
  const kids = node.children && node.children.length > 0;
  const wrap = el('div', 'node-wrap');

  const btn = el(kids ? 'button' : 'div', `node-btn lvl${node.level}${!kids ? ' leaf' : ''}`);
  const lbl = el('span');
  lbl.textContent = node.label;
  btn.appendChild(lbl);
  
  if (kids) {
    const ico = el('span', 'toggle-ico');
    ico.textContent = '▼';
    btn.appendChild(ico);
    btn.setAttribute('aria-expanded', 'true');
    
    btn.addEventListener('click', (e) => {
      e.stopPropagation();
      // Alterna a classe 'collapsed' no 'node-wrap'
      wrap.classList.toggle('collapsed');
      const isNowExpanded = !wrap.classList.contains('collapsed');
      btn.setAttribute('aria-expanded', isNowExpanded);
    });
  }
  wrap.appendChild(btn);

  if (!kids) return wrap;

  const area = el('div', 'children-area');
  
  // Linha vertical descendo do pai
  const stem = el('div', 'conn-v');
  stem.style.height = '24px';
  area.appendChild(stem);

  if (node.children.length === 1) {
    area.appendChild(buildNode(node.children[0]));
  } else {
    // Linha horizontal (apenas decorativa, sem lógica de espaçamento complexa)
    const hLine = el('div', 'conn-h-line');
    hLine.style.height = '1.5px';
    hLine.style.background = '#999';
    hLine.style.margin = '0 20px 0 20px'; // Margens laterais para não ir até as bordas
    area.appendChild(hLine);
    
    // Container das colunas
    const row = el('div', 'children-row', { alignItems: 'flex-start', justifyContent: 'center' });
    
    node.children.forEach((child, idx) => {
      const col = el('div', 'child-col');
      const vdrop = el('div', 'conn-v');
      vdrop.style.height = '20px';
      col.appendChild(vdrop);
      col.appendChild(buildNode(child));
      row.appendChild(col);
      
      // Adiciona um espaçador fixo e visível entre as colunas
      if (idx < node.children.length - 1) {
        const spacer = el('div', 'spacer');
        spacer.style.width = '40px'; // Largura fixa
        spacer.style.flexShrink = '0';
        row.appendChild(spacer);
      }
    });
    area.appendChild(row);
  }

  wrap.appendChild(area);
  return wrap;
}

// --- Inicialização ---
const root = document.getElementById('org-root');
root.innerHTML = ''; // Limpa o conteúdo anterior
const tree = el('div', 'tree');
tree.appendChild(buildNode(org));
root.appendChild(tree);
</script>
