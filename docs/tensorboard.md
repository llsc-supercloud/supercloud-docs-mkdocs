# Tensorboard

 You can run Tensorboard as a job, this is the preferred method of
  doing this.
 
First start an interactive session with a reserved port:

```bash
[studentx@login-1 ~]$ LLsub -i --resv-ports 1           
salloc --immediate=60 -p normal --constraint=xeon-e5 --cpus-per-task=4 --qos=high  srun --resv-ports=1 --pty bash -i     
salloc: Granted job allocation 355286     
salloc: Waiting for resource configuration     
salloc: Nodes node-052 are ready for job      `
```

Then create your logging directory in TMPDIR:

> `[studentx@node-052 ~]$ mkdir -p ${TMPDIR}/tensorboard      `

Set up your forwarding name and file:

```bash
[studentx@node-052 ~]$ PORTAL_FWNAME="$(id -un tr '[A-Z]' '[a-z]')-tensorboard"     
[studentx@node-052 ~]$ PORTAL_FWFILE="/home/gridsan/portal-url-fw/${PORTAL_FWNAME}"     
[studentx@node-052 ~]$ echo $PORTAL_FWFILE     
/home/gridsan/portal-url-fw/studentx-tensorboard    

[studentx@node-052 ~]$ echo "Portal URL is: https://${PORTAL_FWNAME}.fn.txe1-portal.mit.edu/"     
Portal URL is: https://studentx-tensorboard.fn.txe1-portal.mit.edu/`
```

Put the forward URL in the forwarding file (when you run "cat
  \$PORTAL_FWFILE" you should only see one line- if you see two or more,
  delete all but the last line):
 
```bash
[studentx@node-052 ~]$ echo "http://$(hostname -s):${SLURM_STEP_RESV_PORTS}/" >> $PORTAL_FWFILE     
[studentx@node-052 ~]$ cat $PORTAL_FWFILE     
http://node-052:12637/
```

Set the permissions on the forward file properly:

> `[studentx@node-052 ~]$ chmod u+x ${PORTAL_FWFILE}      `

Load an anaconda module and start tensorboard

```bash
[studentx@node-052 ~]$ module load anaconda/2023a
[studentx@node-052 ~]$ tensorboard --logdir ${TMPDIR}/tensorboard --host "$(hostname -s)" --port ${SLURM_STEP_RESV_PORTS}      
```

In the browser, go to the URL listed above (for example, mine is
<https://studentx-tensorboard.fn.txe1-portal.mit.edu/>)

 
