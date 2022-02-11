<h1>Kubedev - Módulo 16 - Self-Healing</h1>

<p>O Self-Healing do Kubernetes reinicia o pod se for detectado algum problema na aplicação.</p>
<p>Utilizar três recursos:</p>

- LivenessProbes;
- Readiness Probes;
- StartUp Probes.

<h2>Api para teste</h2>

<p>Api pedelogo-catalogo possui endpoints que verificam se aplicação esta saudável ou deixam aplicação inacessivel.</p>

<p>O endpoint /health verifica a saúde do container.</p>

<p>Já o endpoint /read verifica se aplicação está pronta para receber requisição.</p>

<p>O endpoint /unhealth derruba aplicação:</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/unhealth.PNG"></img>

<p>Para simular que aplicação não está pronta é usaudo o endipoint</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/unreadfor.PNG"></img>

<h2>Liveness Probes</h2>

<p>O Liveness Probes checa a saúde  do Container</p>

<p>Após executarmos o endpoint /unhealth a aplicação ficará inacessivel.</p>

<p>Em seguida acessarmos o endpoint /health receberemos um erro.</p>
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

<p>Executar novamente o endpoint /unhealth.</p>

<p>Mas desta vez o pod será reiniciado:</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/pod-reiniciado.png"></img>


<p>Aplicação retornará positio para teste do check point</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/healthy.png"></img>

<p>Pronto para receber requisição:</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/requisicao.png"></img>

<h2>Readiness Probes</h2>

<h2>StartUp Probes</h2>