<h1>Kubedev - Módulo 16 - Self-Healing</h1>

<p>O Self-Healing do Kubernetes reinicia o pod se for detectado algum problema na aplicação.</p>
<p>Utilizar três recursos:</p>

- LivenessProbes;
- Readiness Probes;
- StartUp Probes.

<h2>Api para teste</h2>
<p>Api pedelogo-catalogo possui dois endpoints que deixam a aplicação inacessivel.</p>
<p>O endpoint /health verifica a saúde do container.</p>
<p>Já o endpoint /read verifica se aplicação está pronta para receber requisição.

<p>Para derrubar aplicação</p>
<img src="https://github.com/fabiocaettano/kubedev-modulo17-self-healing/blob/startup-probes/images/unhealth.PNG"></img>
<h2>Liveness Probes</h2>

<h2>Readiness Probes</h2>

<h2>StartUp Probes</h2>