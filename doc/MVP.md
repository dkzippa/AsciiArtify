# AsciiArtify

## Phase3: MVP of Ascii Go App deployed by ArgoCD in Kubernetes cluster locally using k3d
- Phase 1: local k8s dev tools investigation : [Concept.md](Concept.md)
- Phase 2: preparation and setup phase: [POC.md](POC.md)

##### Demo recording:
![Image](argo_mvp.gif)

##### check cluster is working, existing namespaces and opened ports:
```
docker ps
k3d cluster list
k get ns
k get svc
ss -tulnp
```

##### check ArgoCD is running?
- open localhost:8080 in browser, must be 'This site can’t be reached'
- `k port-forward svc/argocd-server -n argocd 8080:443`
- open localhost:8080 in browser again
- login with `admin` and your password received in [POC.md](POC.md)
- create the app with required settings and use repository https://github.com/den-vasyliev/go-demo-app as the source
- use namespace `ascii-go-mvp`


##### check all is working correctly with example image:
<img src="assets/argo_logo.png" alt="drawing" width="100"/>

```
k get ns
k get all -n ascii-go-mvp
k port-forward -n ascii-go svc/ambassador 8088:80

curl localhost:8088
wget -O argo_logo.png https://github.com/cncf/artwork/raw/main/projects/argo/stacked/color/argo-stacked-color.png
curl -F 'image=@argo_logo.png' localhost:8088/img/
```


##### here we go, our MVP is working correctly and deployed using GitOps way

                                        .,;itfLCCG000000GCCLft1;:.
                                   .:1fC088@@@@@@@@@88@@@@@@@@@880GL1;,
                               .:tC08@@@@888888888888888888888888@@@@80Ct;.
                            .;fG8@@@8888888888888888888888888888888888@@@80Li.
                          :tG8@@88888888@@88888888888888888888888888888888@@@0f;
                        ;L8@@888888888@@@@@8888888888888888888888888888888888@@8Ci.
                      ;C8@88888888@@@888@888888888888888888888888888888888888888@8Gi
                    ,L8@88888888@@@88888888888888888888888888888888888888888888888@8C;
                   10@8888888@@@888888888888888000GGGGGG0008888888888888888888888888@8f.
                 .L888888888@@88888888888880GCLffffffffffffLCG0888888888888888888888888G:
                :G88888888@@8888888888880GLfffffffffffffffffttfLC088888888888888888888880i
               :088888888@@888888888880CfffffffffffffffffffffffftfL088888888888888888888881
              ,08888888@@888888888888GfffffLLLLffffffffffffffffffftfC8888888888888888888888i
             .G8888888@@888888888888CffffffLLLLLfffffffffffffffffffftL0888888888888888888880:
             f8888888@@888888888888CtfffffffLLffffffffffffffffffffffftf088888888888888888888G.
            :8888888@@888888888888GffffffffffffffffffffffffffffffffffftL888888888888888888888t
         :LfC8888888@@888888888888LfffffffffffffffffffffffffffffffffffftG88888888888888888888GfLi
         f800888888@@8888888888880ffffffffffffffffffffffffffffffffffffftC88888888888888888888008G
         f0G0888888@@888888888888GfftttttttfffffffffffffffffffttttttttftL888888888888888888888G0C
         f0G0888888@88888888880CftfCGG00GGCLftfffffffffffffftfLGG000GCLftfC088888888888888888800C
         f0G088888@@888888888CftL0@@@@@@@@@@0CfffffffffffftL0@@@@@@@@@@8CftfG8888888888888888800C
         f0G088888@@88888888Ctf0@@@@@0CG8@@@@@0fffffffffffG@@@@@@GC0@@@@@8LttG888888888888888800C
         f0G088888@@8888888Gff0@@@@@1.  ,C@@@@@0fffffffftG@@@@@0,   ;8@@@@8fff088888888888888800C
         f0G0888888@8888888CtL@@@@@@:    t@@@@@@Ltfffffff8@@@@@C    .0@@@@@CtfG888888888888888G0C
         f800888888@@888888Ctf@@@@@@8LttG@@@@@@@Ltfffffff8@@@@@@0ftf8@@@@@@CtfG88888888888888000G
         ;CLC888888@@8888880ftG@@@@@@@@@@@@@@@@0fffffffftC@@@@@@@@@@@@@@@@8fff088888888888888GLC1
            :8888888@@888888GftG@@@@@@@@@@@@@@0fffffffffftC@@@@@@@@@@@@@@0ftf08888888888888881
             f8888888@@888888GLtLG8@@@@@@@@@0LtffffffffffftfG8@@@@@@@@@0CttL0888888888888888G.
             .G8888888@88888888GLffLCGGGGCLftffffffffffffffftfLCGGGGCLftfLG88888888888888880:
              ,08888888@@888888880Ltftttttfffffffffffffffffffffttttttfff0888888888888888888i
               :08888888@@88888888CffffffffffffffffffffffffffffffffffffL8888888888888888881
                :G8888888@@8888888CffffffffffffffffffffffffffffffffffffL88888888888888880i
                 .L888888888888888CffffffffffftffffffffffffffffffffffffL888888888888888G:
                   10@888888888888GffffffffffLGfttfffffttfL0fffffffffffC888888888888@8f.
                    ,L8@8888888888Gffffffffff1C@8GCLLLCG0@@ftffffffffffC8888888888@8C;
                      ;C8@88888888Gfffffffffff,1G@@@@@@@8C:;fffffffffffG88888888@8Gi
                        ;L8@@8888@0ffffffffffft  .:;i;;,. :ffffffffffff0@8888@@8Ci.
                          :tG8@@880fffffffffffft:       .ifffffffffffff088@@@0f;
                            .;fG8@8fffffffffffffft1i;;i1fffffffffffffff8@80Li.
                               .:tLffffffffffffffffffffffffffffffffffffCf;.
                                  .ffffffffffffffffffffffffffffffffffff,
                                .;tfffffffffffffffffffffffffffffffffffffi,
                           .:;itfffffffffffffffffffffffffffffffffffffffffft1;:,
                           .,::::,:1ffffffffffffffffffffffffffffffffff1:,,,:,,.
                                   .ffffftfffffffffffffffffffffftfffff:
                                   .fffffiffffffffffffffffffffffifffff,
                                    tffff;fffftfffffffffffftffff;tffff,
                                    1ffft,ffffiffffffffffffitfff:1ffff.
                                    ifff1.ffff;ffffffffffff;tfff,iff:t
                                    ;fffi.ffft,ffffffffffff:1fff,;fff1
                                    :fff; fff1.ffffffffffff,ifff,,fff;
                                   .ifff: tffi.fffff1ifffff,:fff..fff1,.
                             .,:i1tfft1:  tff; fffff, fffff,,fff, ,i1tt11i:,.
                              .....,,,,,;tfft. tfff1  ;ffff. 1fffi,..........
                                 ,::;;iii;:,  ;ffff,   tfff;   ,:;;;;::,.
                                       ..,:;1fffti.    .itfft1;:,..
                                     .,::;;;::,.          ,,::;;;;::,.


