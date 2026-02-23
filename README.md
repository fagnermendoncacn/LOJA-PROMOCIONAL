index.html
 1	<!DOCTYPE html>
     2	<html lang="pt-BR">
     3	<head>
     4	    <meta charset="UTF-8">
     5	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
     6	    <title>Sistema de Estoque - Versão Código de Barras</title>
     7	    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
     8	    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
     9	    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.31/jspdf.plugin.autotable.min.js"></script>
    10	    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    11	    <style>
    12	        * {
    13	            margin: 0;
    14	            padding: 0;
    15	            box-sizing: border-box;
    16	        }
    17	
    18	        :root {
    19	            --primary-blue: #1e40af;
    20	            --medium-blue: #3b82f6;
    21	            --light-blue: #60a5fa;
    22	            --gold: #fbbf24;
    23	            --dark-gold: #f59e0b;
    24	            --white: #ffffff;
    25	            --light-gray: #f3f4f6;
    26	            --gray: #6b7280;
    27	        }
    28	
    29	        body {
    30	            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    31	            background: linear-gradient(135deg, #1e40af 0%, #3b82f6 100%);
    32	            min-height: 100vh;
    33	        }
    34	
    35	        /* Login Screen */
    36	        .login-screen {
    37	            display: flex;
    38	            justify-content: center;
    39	            align-items: center;
    40	            min-height: 100vh;
    41	            padding: 20px;
    42	        }
    43	
    44	        .login-container {
    45	            background: white;
    46	            border-radius: 20px;
    47	            padding: 50px;
    48	            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    49	            max-width: 500px;
    50	            width: 100%;
    51	            text-align: center;
    52	        }
    53	
    54	        .login-container h1 {
    55	            color: var(--primary-blue);
    56	            font-size: 2.5em;
    57	            margin-bottom: 15px;
    58	        }
    59	
    60	        .login-subtitle {
    61	            color: var(--gray);
    62	            font-size: 1.2em;
    63	            margin-bottom: 40px;
    64	        }
    65	
    66	        .quick-login-btn {
    67	            width: 100%;
    68	            padding: 20px;
    69	            margin: 15px 0;
    70	            font-size: 1.3em;
    71	            font-weight: bold;
    72	            border: none;
    73	            border-radius: 15px;
    74	            cursor: pointer;
    75	            transition: all 0.3s;
    76	            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    77	        }
    78	
    79	        .admin-btn {
    80	            background: linear-gradient(135deg, #dc2626 0%, #ef4444 100%);
    81	            color: white;
    82	        }
    83	
    84	        .manager-btn {
    85	            background: linear-gradient(135deg, #d97706 0%, #f59e0b 100%);
    86	            color: white;
    87	        }
    88	
    89	        .seller-btn {
    90	            background: linear-gradient(135deg, #059669 0%, #10b981 100%);
    91	            color: white;
    92	        }
    93	
    94	        .quick-login-btn:hover {
    95	            transform: translateY(-3px);
    96	            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
    97	        }
    98	
    99	        /* Main App Container */
   100	        .app-container {
   101	            display: none;
   102	        }
   103	
   104	        .app-header {
   105	            background: white;
   106	            padding: 20px 40px;
   107	            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
   108	            display: flex;
   109	            justify-content: space-between;
   110	            align-items: center;
   111	        }
   112	
   113	        .app-header h1 {
   114	            color: var(--primary-blue);
   115	            font-size: 1.8em;
   116	        }
   117	
   118	        .user-info {
   119	            display: flex;
   120	            align-items: center;
   121	            gap: 15px;
   122	        }
   123	
   124	        .user-badge {
   125	            background: linear-gradient(135deg, var(--gold), var(--dark-gold));
   126	            color: white;
   127	            padding: 10px 20px;
   128	            border-radius: 25px;
   129	            font-weight: bold;
   130	        }
   131	
   132	        .logout-btn {
   133	            background: var(--primary-blue);
   134	            color: white;
   135	            border: none;
   136	            padding: 10px 20px;
   137	            border-radius: 10px;
   138	            cursor: pointer;
   139	            font-weight: bold;
   140	        }
   141	
   142	        .logout-btn:hover {
   143	            background: var(--medium-blue);
   144	        }
   145	
   146	        .main-content {
   147	            display: flex;
   148	            min-height: calc(100vh - 80px);
   149	        }
   150	
   151	        /* Sidebar */
   152	        .sidebar {
   153	            width: 280px;
   154	            background: white;
   155	            padding: 30px 0;
   156	            box-shadow: 2px 0 10px rgba(0,0,0,0.1);
   157	        }
   158	
   159	        .menu-item {
   160	            padding: 18px 30px;
   161	            cursor: pointer;
   162	            transition: all 0.3s;
   163	            font-size: 1.1em;
   164	            display: flex;
   165	            align-items: center;
   166	            gap: 15px;
   167	            color: var(--gray);
   168	        }
   169	
   170	        .menu-item:hover {
   171	            background: var(--light-gray);
   172	            color: var(--primary-blue);
   173	            border-left: 5px solid var(--gold);
   174	        }
   175	
   176	        .menu-item.active {
   177	            background: linear-gradient(90deg, #eff6ff 0%, #dbeafe 100%);
   178	            color: var(--primary-blue);
   179	            border-left: 5px solid var(--primary-blue);
   180	            font-weight: bold;
   181	        }
   182	
   183	        /* Content Area */
   184	        .content-area {
   185	            flex: 1;
   186	            padding: 40px;
   187	            background: var(--light-gray);
   188	        }
   189	
   190	        .section {
   191	            display: none;
   192	            background: white;
   193	            padding: 40px;
   194	            border-radius: 20px;
   195	            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
   196	        }
   197	
   198	        .section.active {
   199	            display: block;
   200	        }
   201	
   202	        .section-title {
   203	            color: var(--primary-blue);
   204	            font-size: 2em;
   205	            margin-bottom: 30px;
   206	            padding-bottom: 15px;
   207	            border-bottom: 3px solid var(--gold);
   208	        }
   209	
   210	        /* Forms */
   211	        .form-group {
   212	            margin-bottom: 25px;
   213	        }
   214	
   215	        .form-group label {
   216	            display: block;
   217	            color: var(--primary-blue);
   218	            font-weight: bold;
   219	            margin-bottom: 8px;
   220	            font-size: 1.1em;
   221	        }
   222	
   223	        .form-group input, .form-group select, .form-group textarea {
   224	            width: 100%;
   225	            padding: 15px;
   226	            border: 2px solid #e5e7eb;
   227	            border-radius: 10px;
   228	            font-size: 1em;
   229	            transition: all 0.3s;
   230	        }
   231	
   232	        .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
   233	            outline: none;
   234	            border-color: var(--medium-blue);
   235	            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
   236	        }
   237	
   238	        .barcode-input {
   239	            background: #fef3c7 !important;
   240	            border-color: var(--gold) !important;
   241	            font-weight: bold;
   242	            font-size: 1.2em !important;
   243	        }
   244	
   245	        .btn {
   246	            padding: 15px 30px;
   247	            border: none;
   248	            border-radius: 10px;
   249	            font-size: 1.1em;
   250	            font-weight: bold;
   251	            cursor: pointer;
   252	            transition: all 0.3s;
   253	            margin-right: 10px;
   254	        }
   255	
   256	        .btn-primary {
   257	            background: linear-gradient(135deg, var(--primary-blue), var(--medium-blue));
   258	            color: white;
   259	        }
   260	
   261	        .btn-primary:hover {
   262	            transform: translateY(-2px);
   263	            box-shadow: 0 5px 15px rgba(30, 64, 175, 0.4);
   264	        }
   265	
   266	        .btn-gold {
   267	            background: linear-gradient(135deg, var(--gold), var(--dark-gold));
   268	            color: white;
   269	        }
   270	
   271	        .btn-gold:hover {
   272	            transform: translateY(-2px);
   273	            box-shadow: 0 5px 15px rgba(245, 158, 11, 0.4);
   274	        }
   275	
   276	        /* Tables */
   277	        table {
   278	            width: 100%;
   279	            border-collapse: collapse;
   280	            margin-top: 20px;
   281	        }
   282	
   283	        thead {
   284	            background: linear-gradient(135deg, var(--primary-blue), var(--medium-blue));
   285	            color: white;
   286	        }
   287	
   288	        th, td {
   289	            padding: 15px;
   290	            text-align: left;
   291	        }
   292	
   293	        tbody tr:nth-child(even) {
   294	            background: var(--light-gray);
   295	        }
   296	
   297	        tbody tr:hover {
   298	            background: #dbeafe;
   299	        }
   300	
   301	        /* Dashboard Cards */
   302	        .dashboard-cards {
   303	            display: grid;
   304	            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
   305	            gap: 25px;
   306	            margin-bottom: 40px;
   307	        }
   308	
   309	        .dashboard-card {
   310	            background: linear-gradient(135deg, var(--primary-blue), var(--medium-blue));
   311	            color: white;
   312	            padding: 30px;
   313	            border-radius: 20px;
   314	            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
   315	        }
   316	
   317	        .dashboard-card h3 {
   318	            font-size: 1.2em;
   319	            margin-bottom: 15px;
   320	            opacity: 0.9;
   321	        }
   322	
   323	        .dashboard-card .value {
   324	            font-size: 2.5em;
   325	            font-weight: bold;
   326	        }
   327	
   328	        .gold-card {
   329	            background: linear-gradient(135deg, var(--gold), var(--dark-gold)) !important;
   330	        }
   331	
   332	        /* Barcode Scanner Alert */
   333	        .barcode-alert {
   334	            background: linear-gradient(135deg, #fef3c7, #fde68a);
   335	            border-left: 5px solid var(--gold);
   336	            padding: 20px;
   337	            border-radius: 10px;
   338	            margin-bottom: 20px;
   339	            font-weight: bold;
   340	            color: #92400e;
   341	        }
   342	
   343	        .report-filters {
   344	            display: flex;
   345	            gap: 15px;
   346	            margin-bottom: 30px;
   347	            flex-wrap: wrap;
   348	        }
   349	
   350	        .report-filters input {
   351	            flex: 1;
   352	            min-width: 200px;
   353	        }
   354	
   355	        .alert {
   356	            padding: 15px;
   357	            border-radius: 10px;
   358	            margin-bottom: 20px;
   359	            font-weight: bold;
   360	        }
   361	
   362	        .alert-success {
   363	            background: #d1fae5;
   364	            color: #065f46;
   365	            border-left: 5px solid #10b981;
   366	        }
   367	
   368	        .alert-error {
   369	            background: #fee2e2;
   370	            color: #991b1b;
   371	            border-left: 5px solid #ef4444;
   372	        }
   373	
   374	        @media (max-width: 768px) {
   375	            .main-content {
   376	                flex-direction: column;
   377	            }
   378	            .sidebar {
   379	                width: 100%;
   380	            }
   381	            .content-area {
   382	                padding: 20px;
   383	            }
   384	        }
   385	    </style>
   386	</head>
   387	<body>
   388	
   389	<!-- Login Screen -->
   390	<div class="login-screen" id="loginScreen">
   391	    <div class="login-container">
   392	        <h1>🏪 Sistema de Estoque</h1>
   393	        <p class="login-subtitle">Versão Código de Barras Pro</p>
   394	        
   395	        <div class="barcode-alert">
   396	            ⚠️ Sistema otimizado para uso com leitor de código de barras
   397	        </div>
   398	
   399	        <button class="quick-login-btn admin-btn" onclick="quickLogin('admin')">
   400	            🔴 ADMINISTRADOR
   401	        </button>
   402	        <button class="quick-login-btn manager-btn" onclick="quickLogin('gerente')">
   403	            🟡 GERENTE
   404	        </button>
   405	        <button class="quick-login-btn seller-btn" onclick="quickLogin('vendedor')">
   406	            🟢 VENDEDOR
   407	        </button>
   408	    </div>
   409	</div>
   410	
   411	<!-- Main Application -->
   412	<div class="app-container" id="appContainer">
   413	    <div class="app-header">
   414	        <h1>🏪 Sistema de Estoque Pro</h1>
   415	        <div class="user-info">
   416	            <span class="user-badge" id="userBadge">Usuário</span>
   417	            <button class="logout-btn" onclick="logout()">Sair</button>
   418	        </div>
   419	    </div>
   420	
   421	    <div class="main-content">
   422	        <div class="sidebar">
   423	            <div class="menu-item active" onclick="showSection('dashboard')">
   424	                📊 Dashboard
   425	            </div>
   426	            <div class="menu-item" onclick="showSection('produtos')">
   427	                📦 Produtos
   428	            </div>
   429	            <div class="menu-item" onclick="showSection('venda-rapida')">
   430	                🛒 Venda Rápida
   431	            </div>
   432	            <div class="menu-item" onclick="showSection('relatorio-diario')">
   433	                📅 Relatório Diário
   434	            </div>
   435	            <div class="menu-item" onclick="showSection('relatorio-geral')">
   436	                📈 Relatórios Gerais
   437	            </div>
   438	            <div class="menu-item" onclick="showSection('fornecedores')">
   439	                🏢 Fornecedores
   440	            </div>
   441	        </div>
   442	
   443	        <div class="content-area">
   444	            <!-- Dashboard -->
   445	            <div class="section active" id="dashboard">
   446	                <h2 class="section-title">Dashboard</h2>
   447	                <div class="dashboard-cards">
   448	                    <div class="dashboard-card">
   449	                        <h3>Total de Produtos</h3>
   450	                        <div class="value" id="totalProdutos">0</div>
   451	                    </div>
   452	                    <div class="dashboard-card gold-card">
   453	                        <h3>Vendas Hoje</h3>
   454	                        <div class="value" id="vendasHoje">R$ 0,00</div>
   455	                    </div>
   456	                    <div class="dashboard-card">
   457	                        <h3>Vendas no Mês</h3>
   458	                        <div class="value" id="vendasMes">R$ 0,00</div>
   459	                    </div>
   460	                    <div class="dashboard-card gold-card">
   461	                        <h3>Lucro Hoje</h3>
   462	                        <div class="value" id="lucroHoje">R$ 0,00</div>
   463	                    </div>
   464	                </div>
   465	                <canvas id="chartVendas" style="max-height: 400px;"></canvas>
   466	            </div>
   467	
   468	            <!-- Produtos -->
   469	            <div class="section" id="produtos">
   470	                <h2 class="section-title">Gerenciar Produtos</h2>
   471	                
   472	                <div class="barcode-alert">
   473	                    🔍 Use o leitor de código de barras para preencher automaticamente
   474	                </div>
   475	
   476	                <form id="formProduto" onsubmit="salvarProduto(event)">
   477	                    <div class="form-group">
   478	                        <label>🔢 Código de Barras *</label>
   479	                        <input type="text" id="codigoBarras" class="barcode-input" required 
   480	                               placeholder="Escaneie ou digite o código de barras" autofocus>
   481	                    </div>
   482	                    <div class="form-group">
   483	                        <label>Nome do Produto *</label>
   484	                        <input type="text" id="nomeProduto" required>
   485	                    </div>
   486	                    <div class="form-group">
   487	                        <label>Preço de Custo (R$) *</label>
   488	                        <input type="number" id="precoCusto" step="0.01" required>
   489	                    </div>
   490	                    <div class="form-group">
   491	                        <label>Preço de Venda (R$) *</label>
   492	                        <input type="number" id="precoVenda" step="0.01" required>
   493	                    </div>
   494	                    <div class="form-group">
   495	                        <label>Quantidade em Estoque *</label>
   496	                        <input type="number" id="quantidadeProduto" required>
   497	                    </div>
   498	                    <div class="form-group">
   499	                        <label>Estoque Mínimo</label>
   500	                        <input type="number" id="estoqueMinimo" value="5">
   501	                    </div>
   502	                    <div class="form-group">
   503	                        <label>Fornecedor</label>
   504	                        <select id="fornecedorProduto">
   505	                            <option value="">Selecione...</option>
   506	                        </select>
   507	                    </div>
   508	                    <button type="submit" class="btn btn-primary">Salvar Produto</button>
   509	                    <button type="button" class="btn btn-gold" onclick="limparFormProduto()">Limpar</button>
   510	                </form>
   511	
   512	                <h3 style="margin-top: 40px; color: var(--primary-blue);">Lista de Produtos</h3>
   513	                <input type="text" id="buscaProduto" placeholder="🔍 Buscar produto..." 
   514	                       style="width: 100%; padding: 15px; margin: 20px 0; border: 2px solid #e5e7eb; border-radius: 10px;"
   515	                       onkeyup="filtrarProdutos()">
   516	                
   517	                <table id="tabelaProdutos">
   518	                    <thead>
   519	                        <tr>
   520	                            <th>Código Barras</th>
   521	                            <th>Nome</th>
   522	                            <th>Preço Venda</th>
   523	                            <th>Estoque</th>
   524	                            <th>Status</th>
   525	                            <th>Ações</th>
   526	                        </tr>
   527	                    </thead>
   528	                    <tbody id="listaProdutos"></tbody>
   529	                </table>
   530	            </div>
   531	
   532	            <!-- Venda Rápida -->
   533	            <div class="section" id="venda-rapida">
   534	                <h2 class="section-title">🛒 Venda Rápida com Código de Barras</h2>
   535	                
   536	                <div class="barcode-alert">
   537	                    ⚡ Escaneie o código de barras para adicionar produtos à venda
   538	                </div>
   539	
   540	                <div class="form-group">
   541	                    <label>🔢 Código de Barras</label>
   542	                    <input type="text" id="codigoVenda" class="barcode-input" 
   543	                           placeholder="Escaneie o código de barras" 
   544	                           onkeypress="adicionarProdutoVenda(event)" autofocus>
   545	                </div>
   546	
   547	                <div id="itensVenda" style="margin: 30px 0;"></div>
   548	
   549	                <div style="background: #f3f4f6; padding: 20px; border-radius: 10px; margin: 20px 0;">
   550	                    <h3 style="color: var(--primary-blue);">Total da Venda: <span id="totalVenda">R$ 0,00</span></h3>
   551	                    <h3 style="color: var(--gold);">Lucro Estimado: <span id="lucroVenda">R$ 0,00</span></h3>
   552	                </div>
   553	
   554	                <div class="form-group">
   555	                    <label>Cliente (opcional)</label>
   556	                    <input type="text" id="clienteVenda" placeholder="Nome do cliente">
   557	                </div>
   558	
   559	                <button class="btn btn-primary" onclick="finalizarVenda()" style="font-size: 1.3em; padding: 20px 40px;">
   560	                    ✅ FINALIZAR VENDA
   561	                </button>
   562	                <button class="btn btn-gold" onclick="cancelarVenda()">❌ Cancelar</button>
   563	            </div>
   564	
   565	            <!-- Relatório Diário -->
   566	            <div class="section" id="relatorio-diario">
   567	                <h2 class="section-title">📅 Relatório de Vendas Diário</h2>
   568	                
   569	                <div class="report-filters">
   570	                    <input type="date" id="dataRelatorio" class="form-control">
   571	                    <button class="btn btn-primary" onclick="gerarRelatorioDiario()">📊 Gerar Relatório</button>
   572	                    <button class="btn btn-gold" onclick="exportarRelatorioDiarioExcel()">📥 Exportar Excel</button>
   573	                    <button class="btn btn-gold" onclick="exportarRelatorioDiarioPDF()">📄 Exportar PDF</button>
   574	                </div>
   575	
   576	                <div id="resumoDiario" style="margin: 30px 0;"></div>
   577	
   578	                <table id="tabelaVendasDiario">
   579	                    <thead>
   580	                        <tr>
   581	                            <th>Hora</th>
   582	                            <th>Produtos</th>
   583	                            <th>Cliente</th>
   584	                            <th>Total</th>
   585	                            <th>Lucro</th>
   586	                        </tr>
   587	                    </thead>
   588	                    <tbody id="listaVendasDiario"></tbody>
   589	                </table>
   590	            </div>
   591	
   592	            <!-- Relatório Geral -->
   593	            <div class="section" id="relatorio-geral">
   594	                <h2 class="section-title">📈 Relatórios Gerais</h2>
   595	                
   596	                <div class="report-filters">
   597	                    <input type="date" id="dataInicio">
   598	                    <input type="date" id="dataFim">
   599	                    <button class="btn btn-primary" onclick="gerarRelatorioGeral()">📊 Gerar</button>
   600	                    <button class="btn btn-gold" onclick="exportarRelatorioExcel()">📥 Excel</button>
   601	                    <button class="btn btn-gold" onclick="exportarRelatorioPDF()">📄 PDF</button>
   602	                </div>
   603	
   604	                <div id="resumoGeral" style="margin: 30px 0;"></div>
   605	                <canvas id="chartRelatorio" style="max-height: 400px; margin: 30px 0;"></canvas>
   606	
   607	                <h3 style="color: var(--primary-blue); margin: 30px 0;">Produtos Mais Vendidos</h3>
   608	                <table id="tabelaMaisVendidos">
   609	                    <thead>
   610	                        <tr>
   611	                            <th>Produto</th>
   612	                            <th>Quantidade</th>
   613	                            <th>Total Vendido</th>
   614	                        </tr>
   615	                    </thead>
   616	                    <tbody id="listaMaisVendidos"></tbody>
   617	                </table>
   618	            </div>
   619	
   620	            <!-- Fornecedores -->
   621	            <div class="section" id="fornecedores">
   622	                <h2 class="section-title">Gerenciar Fornecedores</h2>
   623	                
   624	                <form id="formFornecedor" onsubmit="salvarFornecedor(event)">
   625	                    <div class="form-group">
   626	                        <label>Nome da Empresa *</label>
   627	                        <input type="text" id="nomeFornecedor" required>
   628	                    </div>
   629	                    <div class="form-group">
   630	                        <label>CNPJ</label>
   631	                        <input type="text" id="cnpjFornecedor">
   632	                    </div>
   633	                    <div class="form-group">
   634	                        <label>Contato</label>
   635	                        <input type="text" id="contatoFornecedor">
   636	                    </div>
   637	                    <div class="form-group">
   638	                        <label>Telefone</label>
   639	                        <input type="tel" id="telefoneFornecedor">
   640	                    </div>
   641	                    <button type="submit" class="btn btn-primary">Salvar Fornecedor</button>
   642	                </form>
   643	
   644	                <table style="margin-top: 30px;">
   645	                    <thead>
   646	                        <tr>
   647	                            <th>Empresa</th>
   648	                            <th>CNPJ</th>
   649	                            <th>Contato</th>
   650	                            <th>Telefone</th>
   651	                        </tr>
   652	                    </thead>
   653	                    <tbody id="listaFornecedores"></tbody>
   654	                </table>
   655	            </div>
   656	        </div>
   657	    </div>
   658	</div>
   659	
   660	<script>
   661	// Dados globais
   662	let currentUser = null;
   663	let produtos = JSON.parse(localStorage.getItem('produtos')) || [];
   664	let vendas = JSON.parse(localStorage.getItem('vendas')) || [];
   665	let fornecedores = JSON.parse(localStorage.getItem('fornecedores')) || [];
   666	let carrinhoVenda = [];
   667	
   668	// Login
   669	function quickLogin(tipo) {
   670	    currentUser = { nome: tipo.toUpperCase(), tipo: tipo };
   671	    document.getElementById('loginScreen').style.display = 'none';
   672	    document.getElementById('appContainer').style.display = 'block';
   673	    document.getElementById('userBadge').textContent = currentUser.nome;
   674	    atualizarDashboard();
   675	    carregarProdutos();
   676	    carregarFornecedores();
   677	    atualizarSelectFornecedores();
   678	    
   679	    // Definir data de hoje no relatório diário
   680	    document.getElementById('dataRelatorio').valueAsDate = new Date();
   681	}
   682	
   683	function logout() {
   684	    currentUser = null;
   685	    document.getElementById('loginScreen').style.display = 'flex';
   686	    document.getElementById('appContainer').style.display = 'none';
   687	}
   688	
   689	// Navegação
   690	function showSection(sectionId) {
   691	    document.querySelectorAll('.section').forEach(section => {
   692	        section.classList.remove('active');
   693	    });
   694	    document.querySelectorAll('.menu-item').forEach(item => {
   695	        item.classList.remove('active');
   696	    });
   697	    document.getElementById(sectionId).classList.add('active');
   698	    event.target.classList.add('active');
   699	    
   700	    // Atualizar dados ao abrir seções específicas
   701	    if (sectionId === 'dashboard') atualizarDashboard();
   702	    if (sectionId === 'relatorio-diario') gerarRelatorioDiario();
   703	}
   704	
   705	// Produtos
   706	function salvarProduto(event) {
   707	    event.preventDefault();
   708	    
   709	    const produto = {
   710	        id: Date.now(),
   711	        codigoBarras: document.getElementById('codigoBarras').value,
   712	        nome: document.getElementById('nomeProduto').value,
   713	        precoCusto: parseFloat(document.getElementById('precoCusto').value),
   714	        precoVenda: parseFloat(document.getElementById('precoVenda').value),
   715	        quantidade: parseInt(document.getElementById('quantidadeProduto').value),
   716	        estoqueMinimo: parseInt(document.getElementById('estoqueMinimo').value),
   717	        fornecedor: document.getElementById('fornecedorProduto').value
   718	    };
   719	    
   720	    produtos.push(produto);
   721	    localStorage.setItem('produtos', JSON.stringify(produtos));
   722	    
   723	    alert('✅ Produto cadastrado com sucesso!');
   724	    limparFormProduto();
   725	    carregarProdutos();
   726	    atualizarDashboard();
   727	}
   728	
   729	function limparFormProduto() {
   730	    document.getElementById('formProduto').reset();
   731	    document.getElementById('codigoBarras').focus();
   732	}
   733	
   734	function carregarProdutos() {
   735	    const tbody = document.getElementById('listaProdutos');
   736	    tbody.innerHTML = '';
   737	    
   738	    produtos.forEach(produto => {
   739	        const status = produto.quantidade <= produto.estoqueMinimo ? 
   740	            '<span style="color: red;">⚠️ Baixo</span>' : 
   741	            '<span style="color: green;">✅ OK</span>';
   742	        
   743	        tbody.innerHTML += `
   744	            <tr>
   745	                <td>${produto.codigoBarras}</td>
   746	                <td>${produto.nome}</td>
   747	                <td>R$ ${produto.precoVenda.toFixed(2)}</td>
   748	                <td>${produto.quantidade}</td>
   749	                <td>${status}</td>
   750	                <td>
   751	                    <button class="btn btn-primary" style="padding: 8px 15px; font-size: 0.9em;" 
   752	                            onclick="editarProduto(${produto.id})">Editar</button>
   753	                </td>
   754	            </tr>
   755	        `;
   756	    });
   757	}
   758	
   759	function filtrarProdutos() {
   760	    const busca = document.getElementById('buscaProduto').value.toLowerCase();
   761	    const linhas = document.querySelectorAll('#listaProdutos tr');
   762	    
   763	    linhas.forEach(linha => {
   764	        const texto = linha.textContent.toLowerCase();
   765	        linha.style.display = texto.includes(busca) ? '' : 'none';
   766	    });
   767	}
   768	
   769	// Venda Rápida
   770	function adicionarProdutoVenda(event) {
   771	    if (event.key === 'Enter') {
   772	        event.preventDefault();
   773	        const codigo = document.getElementById('codigoVenda').value;
   774	        const produto = produtos.find(p => p.codigoBarras === codigo);
   775	        
   776	        if (!produto) {
   777	            alert('❌ Produto não encontrado!');
   778	            return;
   779	        }
   780	        
   781	        if (produto.quantidade <= 0) {
   782	            alert('❌ Produto sem estoque!');
   783	            return;
   784	        }
   785	        
   786	        // Verificar se já está no carrinho
   787	        const itemExistente = carrinhoVenda.find(item => item.id === produto.id);
   788	        if (itemExistente) {
   789	            itemExistente.quantidade++;
   790	        } else {
   791	            carrinhoVenda.push({
   792	                id: produto.id,
   793	                nome: produto.nome,
   794	                preco: produto.precoVenda,
   795	                precoCusto: produto.precoCusto,
   796	                quantidade: 1
   797	            });
   798	        }
   799	        
   800	        atualizarCarrinho();
   801	        document.getElementById('codigoVenda').value = '';
   802	        document.getElementById('codigoVenda').focus();
   803	    }
   804	}
   805	
   806	function atualizarCarrinho() {
   807	    const container = document.getElementById('itensVenda');
   808	    let html = '<table style="width: 100%;"><thead><tr><th>Produto</th><th>Qtd</th><th>Preço Unit.</th><th>Subtotal</th><th>Ação</th></tr></thead><tbody>';
   809	    
   810	    let total = 0;
   811	    let lucro = 0;
   812	    
   813	    carrinhoVenda.forEach((item, index) => {
   814	        const subtotal = item.preco * item.quantidade;
   815	        const lucroItem = (item.preco - item.precoCusto) * item.quantidade;
   816	        total += subtotal;
   817	        lucro += lucroItem;
   818	        
   819	        html += `
   820	            <tr>
   821	                <td>${item.nome}</td>
   822	                <td>${item.quantidade}</td>
   823	                <td>R$ ${item.preco.toFixed(2)}</td>
   824	                <td>R$ ${subtotal.toFixed(2)}</td>
   825	                <td><button onclick="removerItemVenda(${index})" class="btn btn-gold" style="padding: 5px 10px; font-size: 0.9em;">Remover</button></td>
   826	            </tr>
   827	        `;
   828	    });
   829	    
   830	    html += '</tbody></table>';
   831	    container.innerHTML = html;
   832	    
   833	    document.getElementById('totalVenda').textContent = 'R$ ' + total.toFixed(2);
   834	    document.getElementById('lucroVenda').textContent = 'R$ ' + lucro.toFixed(2);
   835	}
   836	
   837	function removerItemVenda(index) {
   838	    carrinhoVenda.splice(index, 1);
   839	    atualizarCarrinho();
   840	}
   841	
   842	function finalizarVenda() {
   843	    if (carrinhoVenda.length === 0) {
   844	        alert('❌ Carrinho vazio!');
   845	        return;
   846	    }
   847	    
   848	    const venda = {
   849	        id: Date.now(),
   850	        data: new Date().toISOString(),
   851	        itens: [...carrinhoVenda],
   852	        cliente: document.getElementById('clienteVenda').value || 'Cliente Anônimo',
   853	        total: parseFloat(document.getElementById('totalVenda').textContent.replace('R$ ', '')),
   854	        lucro: parseFloat(document.getElementById('lucroVenda').textContent.replace('R$ ', ''))
   855	    };
   856	    
   857	    // Atualizar estoque
   858	    carrinhoVenda.forEach(item => {
   859	        const produto = produtos.find(p => p.id === item.id);
   860	        if (produto) {
   861	            produto.quantidade -= item.quantidade;
   862	        }
   863	    });
   864	    
   865	    vendas.push(venda);
   866	    localStorage.setItem('vendas', JSON.stringify(vendas));
   867	    localStorage.setItem('produtos', JSON.stringify(produtos));
   868	    
   869	    alert('✅ Venda finalizada com sucesso!\n\nTotal: R$ ' + venda.total.toFixed(2));
   870	    
   871	    cancelarVenda();
   872	    carregarProdutos();
   873	    atualizarDashboard();
   874	}
   875	
   876	function cancelarVenda() {
   877	    carrinhoVenda = [];
   878	    atualizarCarrinho();
   879	    document.getElementById('clienteVenda').value = '';
   880	    document.getElementById('codigoVenda').focus();
   881	}
   882	
   883	// Relatório Diário
   884	function gerarRelatorioDiario() {
   885	    const dataInput = document.getElementById('dataRelatorio').value;
   886	    if (!dataInput) {
   887	        alert('Selecione uma data!');
   888	        return;
   889	    }
   890	    
   891	    const dataFiltro = new Date(dataInput + 'T00:00:00');
   892	    const vendasDoDia = vendas.filter(venda => {
   893	        const dataVenda = new Date(venda.data);
   894	        return dataVenda.toDateString() === dataFiltro.toDateString();
   895	    });
   896	    
   897	    // Resumo
   898	    const totalVendas = vendasDoDia.reduce((sum, v) => sum + v.total, 0);
   899	    const totalLucro = vendasDoDia.reduce((sum, v) => sum + v.lucro, 0);
   900	    
   901	    document.getElementById('resumoDiario').innerHTML = `
   902	        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px;">
   903	            <div class="dashboard-card">
   904	                <h3>Vendas do Dia</h3>
   905	                <div class="value">${vendasDoDia.length}</div>
   906	            </div>
   907	            <div class="dashboard-card gold-card">
   908	                <h3>Total Vendido</h3>
   909	                <div class="value">R$ ${totalVendas.toFixed(2)}</div>
   910	            </div>
   911	            <div class="dashboard-card">
   912	                <h3>Lucro do Dia</h3>
   913	                <div class="value">R$ ${totalLucro.toFixed(2)}</div>
   914	            </div>
   915	        </div>
   916	    `;
   917	    
   918	    // Tabela
   919	    const tbody = document.getElementById('listaVendasDiario');
   920	    tbody.innerHTML = '';
   921	    
   922	    vendasDoDia.forEach(venda => {
   923	        const hora = new Date(venda.data).toLocaleTimeString('pt-BR');
   924	        const produtos = venda.itens.map(i => `${i.nome} (${i.quantidade}x)`).join(', ');
   925	        
   926	        tbody.innerHTML += `
   927	            <tr>
   928	                <td>${hora}</td>
   929	                <td>${produtos}</td>
   930	                <td>${venda.cliente}</td>
   931	                <td>R$ ${venda.total.toFixed(2)}</td>
   932	                <td style="color: var(--gold); font-weight: bold;">R$ ${venda.lucro.toFixed(2)}</td>
   933	            </tr>
   934	        `;
   935	    });
   936	}
   937	
   938	function exportarRelatorioDiarioExcel() {
   939	    const dataInput = document.getElementById('dataRelatorio').value;
   940	    if (!dataInput) {
   941	        alert('Selecione uma data primeiro!');
   942	        return;
   943	    }
   944	    
   945	    const dataFiltro = new Date(dataInput + 'T00:00:00');
   946	    const vendasDoDia = vendas.filter(venda => {
   947	        const dataVenda = new Date(venda.data);
   948	        return dataVenda.toDateString() === dataFiltro.toDateString();
   949	    });
   950	    
   951	    const dados = vendasDoDia.map(venda => ({
   952	        'Hora': new Date(venda.data).toLocaleTimeString('pt-BR'),
   953	        'Produtos': venda.itens.map(i => `${i.nome} (${i.quantidade}x)`).join(', '),
   954	        'Cliente': venda.cliente,
   955	        'Total': venda.total.toFixed(2),
   956	        'Lucro': venda.lucro.toFixed(2)
   957	    }));
   958	    
   959	    const ws = XLSX.utils.json_to_sheet(dados);
   960	    const wb = XLSX.utils.book_new();
   961	    XLSX.utils.book_append_sheet(wb, ws, "Vendas Diário");
   962	    XLSX.writeFile(wb, `relatorio_diario_${dataInput}.xlsx`);
   963	}
   964	
   965	function exportarRelatorioDiarioPDF() {
   966	    const dataInput = document.getElementById('dataRelatorio').value;
   967	    if (!dataInput) {
   968	        alert('Selecione uma data primeiro!');
   969	        return;
   970	    }
   971	    
   972	    const { jsPDF } = window.jspdf;
   973	    const doc = new jsPDF();
   974	    
   975	    doc.setFontSize(18);
   976	    doc.text('Relatório de Vendas Diário', 14, 20);
   977	    doc.setFontSize(12);
   978	    doc.text(`Data: ${new Date(dataInput + 'T00:00:00').toLocaleDateString('pt-BR')}`, 14, 30);
   979	    
   980	    const dataFiltro = new Date(dataInput + 'T00:00:00');
   981	    const vendasDoDia = vendas.filter(venda => {
   982	        const dataVenda = new Date(venda.data);
   983	        return dataVenda.toDateString() === dataFiltro.toDateString();
   984	    });
   985	    
   986	    const dados = vendasDoDia.map(venda => [
   987	        new Date(venda.data).toLocaleTimeString('pt-BR'),
   988	        venda.itens.map(i => `${i.nome} (${i.quantidade}x)`).join(', '),
   989	        venda.cliente,
   990	        'R$ ' + venda.total.toFixed(2),
   991	        'R$ ' + venda.lucro.toFixed(2)
   992	    ]);
   993	    
   994	    doc.autoTable({
   995	        head: [['Hora', 'Produtos', 'Cliente', 'Total', 'Lucro']],
   996	        body: dados,
   997	        startY: 40
   998	    });
   999	    
  1000	    doc.save(`relatorio_diario_${dataInput}.pdf`);
  1001	}
  1002	
  1003	// Relatório Geral
  1004	function gerarRelatorioGeral() {
  1005	    const dataInicio = new Date(document.getElementById('dataInicio').value);
  1006	    const dataFim = new Date(document.getElementById('dataFim').value);
  1007	    
  1008	    const vendasPeriodo = vendas.filter(venda => {
  1009	        const dataVenda = new Date(venda.data);
  1010	        return dataVenda >= dataInicio && dataVenda <= dataFim;
  1011	    });
  1012	    
  1013	    const totalVendas = vendasPeriodo.reduce((sum, v) => sum + v.total, 0);
  1014	    const totalLucro = vendasPeriodo.reduce((sum, v) => sum + v.lucro, 0);
  1015	    
  1016	    document.getElementById('resumoGeral').innerHTML = `
  1017	        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px;">
  1018	            <div class="dashboard-card">
  1019	                <h3>Total de Vendas</h3>
  1020	                <div class="value">${vendasPeriodo.length}</div>
  1021	            </div>
  1022	            <div class="dashboard-card gold-card">
  1023	                <h3>Total Vendido</h3>
  1024	                <div class="value">R$ ${totalVendas.toFixed(2)}</div>
  1025	            </div>
  1026	            <div class="dashboard-card">
  1027	                <h3>Lucro Total</h3>
  1028	                <div class="value">R$ ${totalLucro.toFixed(2)}</div>
  1029	            </div>
  1030	        </div>
  1031	    `;
  1032	    
  1033	    // Produtos mais vendidos
  1034	    const produtosVendidos = {};
  1035	    vendasPeriodo.forEach(venda => {
  1036	        venda.itens.forEach(item => {
  1037	            if (!produtosVendidos[item.nome]) {
  1038	                produtosVendidos[item.nome] = { quantidade: 0, total: 0 };
  1039	            }
  1040	            produtosVendidos[item.nome].quantidade += item.quantidade;
  1041	            produtosVendidos[item.nome].total += item.preco * item.quantidade;
  1042	        });
  1043	    });
  1044	    
  1045	    const tbody = document.getElementById('listaMaisVendidos');
  1046	    tbody.innerHTML = '';
  1047	    
  1048	    Object.entries(produtosVendidos)
  1049	        .sort((a, b) => b[1].quantidade - a[1].quantidade)
  1050	        .slice(0, 10)
  1051	        .forEach(([nome, dados]) => {
  1052	            tbody.innerHTML += `
  1053	                <tr>
  1054	                    <td>${nome}</td>
  1055	                    <td>${dados.quantidade}</td>
  1056	                    <td>R$ ${dados.total.toFixed(2)}</td>
  1057	                </tr>
  1058	            `;
  1059	        });
  1060	}
  1061	
  1062	function exportarRelatorioExcel() {
  1063	    const dataInicio = new Date(document.getElementById('dataInicio').value);
  1064	    const dataFim = new Date(document.getElementById('dataFim').value);
  1065	    
  1066	    const vendasPeriodo = vendas.filter(venda => {
  1067	        const dataVenda = new Date(venda.data);
  1068	        return dataVenda >= dataInicio && dataVenda <= dataFim;
  1069	    });
  1070	    
  1071	    const dados = vendasPeriodo.map(venda => ({
  1072	        'Data': new Date(venda.data).toLocaleString('pt-BR'),
  1073	        'Cliente': venda.cliente,
  1074	        'Total': venda.total.toFixed(2),
  1075	        'Lucro': venda.lucro.toFixed(2),
  1076	        'Itens': venda.itens.map(i => `${i.nome} (${i.quantidade}x)`).join('; ')
  1077	    }));
  1078	    
  1079	    const ws = XLSX.utils.json_to_sheet(dados);
  1080	    const wb = XLSX.utils.book_new();
  1081	    XLSX.utils.book_append_sheet(wb, ws, "Vendas Geral");
  1082	    XLSX.writeFile(wb, `relatorio_geral.xlsx`);
  1083	}
  1084	
  1085	function exportarRelatorioPDF() {
  1086	    const { jsPDF } = window.jspdf;
  1087	    const doc = new jsPDF();
  1088	    
  1089	    doc.setFontSize(18);
  1090	    doc.text('Relatório Geral de Vendas', 14, 20);
  1091	    
  1092	    const dataInicio = document.getElementById('dataInicio').value;
  1093	    const dataFim = document.getElementById('dataFim').value;
  1094	    doc.setFontSize(12);
  1095	    doc.text(`Período: ${dataInicio} a ${dataFim}`, 14, 30);
  1096	    
  1097	    const vendasPeriodo = vendas.filter(venda => {
  1098	        const dataVenda = new Date(venda.data);
  1099	        return dataVenda >= new Date(dataInicio) && dataVenda <= new Date(dataFim);
  1100	    });
  1101	    
  1102	    const dados = vendasPeriodo.map(venda => [
  1103	        new Date(venda.data).toLocaleString('pt-BR'),
  1104	        venda.cliente,
  1105	        'R$ ' + venda.total.toFixed(2),
  1106	        'R$ ' + venda.lucro.toFixed(2)
  1107	    ]);
  1108	    
  1109	    doc.autoTable({
  1110	        head: [['Data', 'Cliente', 'Total', 'Lucro']],
  1111	        body: dados,
  1112	        startY: 40
  1113	    });
  1114	    
  1115	    doc.save('relatorio_geral.pdf');
  1116	}
  1117	
  1118	// Fornecedores
  1119	function salvarFornecedor(event) {
  1120	    event.preventDefault();
  1121	    
  1122	    const fornecedor = {
  1123	        id: Date.now(),
  1124	        nome: document.getElementById('nomeFornecedor').value,
  1125	        cnpj: document.getElementById('cnpjFornecedor').value,
  1126	        contato: document.getElementById('contatoFornecedor').value,
  1127	        telefone: document.getElementById('telefoneFornecedor').value
  1128	    };
  1129	    
  1130	    fornecedores.push(fornecedor);
  1131	    localStorage.setItem('fornecedores', JSON.stringify(fornecedores));
  1132	    
  1133	    alert('✅ Fornecedor cadastrado!');
  1134	    document.getElementById('formFornecedor').reset();
  1135	    carregarFornecedores();
  1136	    atualizarSelectFornecedores();
  1137	}
  1138	
  1139	function carregarFornecedores() {
  1140	    const tbody = document.getElementById('listaFornecedores');
  1141	    tbody.innerHTML = '';
  1142	    
  1143	    fornecedores.forEach(fornecedor => {
  1144	        tbody.innerHTML += `
  1145	            <tr>
  1146	                <td>${fornecedor.nome}</td>
  1147	                <td>${fornecedor.cnpj || '-'}</td>
  1148	                <td>${fornecedor.contato || '-'}</td>
  1149	                <td>${fornecedor.telefone || '-'}</td>
  1150	            </tr>
  1151	        `;
  1152	    });
  1153	}
  1154	
  1155	function atualizarSelectFornecedores() {
  1156	    const select = document.getElementById('fornecedorProduto');
  1157	    select.innerHTML = '<option value="">Selecione...</option>';
  1158	    
  1159	    fornecedores.forEach(fornecedor => {
  1160	        select.innerHTML += `<option value="${fornecedor.id}">${fornecedor.nome}</option>`;
  1161	    });
  1162	}
  1163	
  1164	// Dashboard
  1165	function atualizarDashboard() {
  1166	    document.getElementById('totalProdutos').textContent = produtos.length;
  1167	    
  1168	    const hoje = new Date();
  1169	    const vendasHoje = vendas.filter(v => {
  1170	        const dataVenda = new Date(v.data);
  1171	        return dataVenda.toDateString() === hoje.toDateString();
  1172	    });
  1173	    
  1174	    const totalHoje = vendasHoje.reduce((sum, v) => sum + v.total, 0);
  1175	    const lucroHoje = vendasHoje.reduce((sum, v) => sum + v.lucro, 0);
  1176	    
  1177	    document.getElementById('vendasHoje').textContent = 'R$ ' + totalHoje.toFixed(2);
  1178	    document.getElementById('lucroHoje').textContent = 'R$ ' + lucroHoje.toFixed(2);
  1179	    
  1180	    const mesAtual = hoje.getMonth();
  1181	    const vendasMes = vendas.filter(v => new Date(v.data).getMonth() === mesAtual);
  1182	    const totalMes = vendasMes.reduce((sum, v) => sum + v.total, 0);
  1183	    
  1184	    document.getElementById('vendasMes').textContent = 'R$ ' + totalMes.toFixed(2);
  1185	}
  1186	
  1187	// Inicialização
  1188	window.onload = function() {
  1189	    // Definir data de hoje nos campos de data
  1190	    const hoje = new Date().toISOString().split('T')[0];
  1191	    document.getElementById('dataRelatorio').value = hoje;
  1192	    document.getElementById('dataInicio').value = hoje;
  1193	    document.getElementById('dataFim').value = hoje;
  1194	};
  1195	</script>
  1196	
  1197	</body>
  1198	</html># LOJA-PROMOCIONAL
