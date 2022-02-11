<h1>Kubedev - Módulo 16 - Self-Healing</h1>

<p>O Self-Healing do Kubernetes reinicia o pod se for detectado algum problema na aplicação.</p>
<p>Utilizar três recursos:</p>

- LivenessProbes;
- Readiness Probes;
- StartUp Probes.

<h2>Api para teste</h2>

<p>Acessar aplicação:</p>

``` http
http://206.189.242.32/swagger/index.html
```

<p>Api pedelogo-catalogo possui endpoints que verificam se aplicação esta saudável ou deixam aplicação inacessivel.</p>

<p>O endpoint /health verifica a saúde do container.</p>

<p>Já o endpoint /read verifica se aplicação está pronta para receber requisição.</p>

<p>O endpoint /unhealth derruba aplicação:</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/master/images/unhealth.PNG"></img>

<p>Para simular que aplicação não está pronta é usaudo o endpoint /unreadfor.</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/master/images/unreadfor.PNG"></img>

<h2>Liveness Probes</h2>

<p>O Liveness Probes checa a saúde  do Container</p>

<p>Após executarmos o endpoint /unhealth a aplicação ficará inacessivel.</p>

<p>O endpoint /health retornará com um erro.</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/unhealth2.png">

<p>Para reinciar o pod é necessário incluir o livenessProbe no manifesto api/deployment.yaml:</p>

``` yaml
livenessProbe:
    httpGet:
    path: /health
    port: 80
    scheme: HTTP
initialDelaySeconds: 1
timeoutSeconds: 1
failureThreshold: 3
periodSeconds: 5
```

Aplicar alteração:

``` bash
$ kubectl apply -f ./ -R
```

<p>Executar novamente o endpoint /unhealth para derrubar aplicação</p>

<p>Com o parametro periodSeconds a livenessProbes irá checar o endpoint a cada 5 segundos.</p>
<p>O parametro failureThreshold define o número limite de tentativas, se alcanção o pod é reiniciado.</p>
<p>O timetourSeconds define o limite de aguardo para retorno do teste de conexão</p>


<p>Quando o POD é reinciado ele demostra na coluna RESTARTS :</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/pod-reiniciado.png"></img>


<p>Após o pod se reiniciado a aplicação retornará positio para teste do check point</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/healthy.png"></img>

<p>Pronto para receber requisição:</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/requisicao.png"></img>

<h2>Readiness Probes</h2>

<p>Incluir no api > deployment.yaml:</p>

``` yaml
readinessProbe:
    httpGet:
        path: /read
        port: 80
        scheme: HTTP
    initialDelaySeconds: 1
    timeoutSeconds: 1
    failureThreshold: 3
    periodSeconds: 10
```

Aplicar alteração:

``` bash
$ kubectl apply -f ./ -R
```


<p>Nesta situação o pod ficará no status de "não pronto".</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/pod-nao-iniciado.png"></p>


<p>Por este motivo o pod não será conectado ao serviço.</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/unbind.png"></img>

<p>Quando o pod for reiniciado o endpoint será vinculado</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/bind.png"></img>


<h2>StartUp Probes</h2>

<p>Configuração indicada para aplicações que levam um certo tempo para inicializar</p>

<p>Incluir no api > deployment.yaml:</p>

``` yaml
startupProbe:
    httpGet:
        path: /health
        port: 80
        scheme: HTTP
    failureThreshold: 50
    periodSeconds: 10
```

Aplicar alteração:

``` bash
$ kubectl apply -f ./ -R
```



