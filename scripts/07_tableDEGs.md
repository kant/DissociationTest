Import file with information about differentially expressed genes.

    dissociation <- read.csv("../results/01_dissociation_volcanoTreatment.csv", header = T, row.names = 1)
    head(dissociation)

    ##            gene     pvalue        lfc      padj color
    ## 1 0610007P14Rik 0.06147768 -0.5281136 0.8680052  none
    ## 2 0610009B22Rik 0.01810797  0.2891053 0.9591621  none
    ## 3 0610009L18Rik 0.01887017 -0.3916629 0.9574803  none
    ## 4 0610009O20Rik 0.03749992  0.2571289 0.9172761  none
    ## 5 0610010F05Rik 0.03987983 -0.2712032 0.9122632  none
    ## 6 0610010K14Rik 0.02680752  0.2983853 0.9401399  none

Filter out non-significant genes, sort by p-value, rename column, round
to 2 decimal places.

    DEGes <- dissociation %>%
      filter(color != "none") %>%
      arrange((padj))
    DEGes$pvalue <- NULL # drop log pvalue columns
    names(DEGes)[4] <- "upregulated in"
    DEGes$lfc <- signif(DEGes$lfc, digits = 2)
    DEGes$padj <- signif(DEGes$padj, digits = 3)
    DEGes

    ##              gene   lfc     padj upregulated in
    ## 1             Trf  2.70 5.30e-07           DISS
    ## 2            Hexb  2.30 8.10e-07           DISS
    ## 3          Selplg  3.00 9.22e-07           DISS
    ## 4            C1qb  2.30 7.08e-06           DISS
    ## 5           Csf1r  2.10 9.58e-06           DISS
    ## 6            Ctss  2.60 9.58e-06           DISS
    ## 7             Cnp  2.50 4.47e-05           DISS
    ## 8            Il1a  3.10 4.47e-05           DISS
    ## 9             Mag  3.30 4.47e-05           DISS
    ## 10           Cd14  3.40 4.88e-05           DISS
    ## 11          Mpeg1  2.40 6.47e-05           DISS
    ## 12        Tmem88b  3.10 6.77e-05           DISS
    ## 13         Nfkbia  2.10 6.87e-05           DISS
    ## 14        Slc15a3  3.40 9.32e-05           DISS
    ## 15           Cd83  2.60 9.98e-05           DISS
    ## 16         Cldn11  3.10 1.05e-04           DISS
    ## 17         Laptm5  2.30 2.23e-04           DISS
    ## 18           Plau  3.90 2.23e-04           DISS
    ## 19           Plp1  2.70 2.23e-04           DISS
    ## 20            Mal  3.20 2.32e-04           DISS
    ## 21           Plek  2.50 2.32e-04           DISS
    ## 22            Jun  1.60 3.20e-04           DISS
    ## 23           Mobp  2.60 4.41e-04           DISS
    ## 24         Pcsk1n  1.30 4.80e-04           DISS
    ## 25          Socs3  2.30 4.80e-04           DISS
    ## 26           Pllp  2.80 5.46e-04           DISS
    ## 27          Ndrg1  2.90 5.79e-04           DISS
    ## 28         Phldb1  2.30 6.84e-04           DISS
    ## 29         Tyrobp  3.30 8.22e-04           DISS
    ## 30         Cx3cr1  2.10 9.50e-04           DISS
    ## 31        Tmem119  2.50 9.50e-04           DISS
    ## 32           C1qa  2.30 9.84e-04           DISS
    ## 33         Slc2a5  4.10 1.23e-03           DISS
    ## 34          Fcrls  2.70 1.40e-03           DISS
    ## 35           Lgmn  1.80 1.57e-03           DISS
    ## 36          Clic4  2.00 1.81e-03           DISS
    ## 37         Sema5a  3.30 2.97e-03           DISS
    ## 38          C5ar1  3.70 3.01e-03           DISS
    ## 39          Cmtm5  2.70 3.01e-03           DISS
    ## 40          Ctcfl  3.10 3.04e-03           DISS
    ## 41          Cryab  2.80 3.07e-03           DISS
    ## 42          Rpl26  1.30 3.22e-03           DISS
    ## 43           Gab1  2.90 3.44e-03           DISS
    ## 44           C1qc  2.60 4.39e-03           DISS
    ## 45           Fa2h  3.30 4.60e-03           DISS
    ## 46         Clec2l  1.90 5.02e-03           DISS
    ## 47          Spry2  1.80 5.07e-03           DISS
    ## 48           Mcam  2.90 5.13e-03           DISS
    ## 49         Olfml3  2.10 5.20e-03           DISS
    ## 50          Sepp1  1.50 5.78e-03           DISS
    ## 51           Ptma  1.20 6.21e-03           DISS
    ## 52         Csrnp3 -1.50 6.58e-03           HOMO
    ## 53          Dusp1  1.30 8.01e-03           DISS
    ## 54           Lag3  3.30 8.01e-03           DISS
    ## 55            Mbp  2.00 8.01e-03           DISS
    ## 56           Atf3  2.20 8.09e-03           DISS
    ## 57          C1ql1  3.10 8.09e-03           DISS
    ## 58         Adgrg1  1.90 8.62e-03           DISS
    ## 59          Csrp1  1.70 9.07e-03           DISS
    ## 60          Ltbp3  2.30 9.07e-03           DISS
    ## 61           Mcl1  1.40 9.07e-03           DISS
    ## 62          Rpl29  1.20 9.07e-03           DISS
    ## 63         Rpl36a  1.60 9.07e-03           DISS
    ## 64        Stxbp5l -1.90 9.07e-03           HOMO
    ## 65       Ighv14-2  7.80 9.59e-03           DISS
    ## 66          Rps27  1.30 9.59e-03           DISS
    ## 67          Mapk3  1.60 9.80e-03           DISS
    ## 68         Gabrb1 -1.10 1.03e-02           HOMO
    ## 69         Rgs7bp -1.40 1.14e-02           HOMO
    ## 70          Trem2  3.30 1.14e-02           DISS
    ## 71         Syngr2  3.40 1.20e-02           DISS
    ## 72           Irf8  2.80 1.26e-02           DISS
    ## 73           Jund  1.10 1.26e-02           DISS
    ## 74        Plekhb1  1.50 1.26e-02           DISS
    ## 75         Grin2b -1.70 1.32e-02           HOMO
    ## 76          Sorl1 -1.40 1.35e-02           HOMO
    ## 77           Cst3  1.50 1.45e-02           DISS
    ## 78          Rpl28  1.20 1.55e-02           DISS
    ## 79          Rps4x  0.98 1.55e-02           DISS
    ## 80        Unc93b1  2.30 1.55e-02           DISS
    ## 81      D6Wsu163e  2.30 1.59e-02           DISS
    ## 82          Sstr1  4.30 1.59e-02           DISS
    ## 83          Birc6 -1.30 1.60e-02           HOMO
    ## 84          Cdkl5 -1.70 1.60e-02           HOMO
    ## 85        Gm10401  7.60 1.60e-02           DISS
    ## 86          Itga5  3.10 1.60e-02           DISS
    ## 87           Ly86  2.80 1.60e-02           DISS
    ## 88          Olig1  1.70 1.60e-02           DISS
    ## 89          Zfp36  1.80 1.60e-02           DISS
    ## 90       Ralgapa1 -1.20 1.63e-02           HOMO
    ## 91           Rps8  1.00 1.70e-02           DISS
    ## 92            Cd9  2.40 1.71e-02           DISS
    ## 93          Rph3a  2.30 1.71e-02           DISS
    ## 94         Slc2a1  1.70 1.77e-02           DISS
    ## 95           Bin2  2.50 1.83e-02           DISS
    ## 96        Arhgef9 -0.93 1.87e-02           HOMO
    ## 97          Gpr84  2.90 1.87e-02           DISS
    ## 98          H2-K1  2.10 1.87e-02           DISS
    ## 99          Rc3h2 -2.00 1.87e-02           HOMO
    ## 100       Rundc3a  0.92 1.87e-02           DISS
    ## 101       Slco2b1  2.20 1.87e-02           DISS
    ## 102        Tgfbr2  2.40 1.87e-02           DISS
    ## 103          Cdh9  3.70 1.99e-02           DISS
    ## 104          Ctsd  1.30 1.99e-02           DISS
    ## 105         Ptgds  2.50 1.99e-02           DISS
    ## 106         Epha6 -1.70 2.00e-02           HOMO
    ## 107           Mt1  1.40 2.00e-02           DISS
    ## 108         Itgb5  2.00 2.01e-02           DISS
    ## 109        Inpp5d  2.80 2.04e-02           DISS
    ## 110         Icam1  2.90 2.06e-02           DISS
    ## 111        Kcnma1 -1.90 2.06e-02           HOMO
    ## 112          Myrf  2.20 2.21e-02           DISS
    ## 113        Nos1ap -1.20 2.21e-02           HOMO
    ## 114         Sparc  1.80 2.21e-02           DISS
    ## 115           Tnf  2.40 2.21e-02           DISS
    ## 116          Rps3  0.87 2.22e-02           DISS
    ## 117       Siglech  2.10 2.22e-02           DISS
    ## 118     Serpinb1a  4.40 2.26e-02           DISS
    ## 119        mt-Co3  1.40 2.26e-02           DISS
    ## 120           Mog  2.50 2.27e-02           DISS
    ## 121         Stat3  1.60 2.41e-02           DISS
    ## 122          Tlr7  3.90 2.42e-02           DISS
    ## 123        Plppr4 -1.20 2.49e-02           HOMO
    ## 124        Celsr2 -1.10 2.54e-02           HOMO
    ## 125        Lgals9  2.70 2.62e-02           DISS
    ## 126        mt-Nd5  0.91 2.62e-02           DISS
    ## 127          Tln1  1.30 2.62e-02           DISS
    ## 128          Cyba  3.80 2.63e-02           DISS
    ## 129          Ermn  2.50 2.69e-02           DISS
    ## 130           Fn1  1.80 2.69e-02           DISS
    ## 131        Grin2a -1.70 2.69e-02           HOMO
    ## 132        mt-Co1  1.20 2.69e-02           DISS
    ## 133         Sipa1  2.70 2.69e-02           DISS
    ## 134         Lrrc7 -1.80 2.73e-02           HOMO
    ## 135          Vasp  2.30 2.73e-02           DISS
    ## 136       Shroom3  6.20 2.80e-02           DISS
    ## 137       mt-Cytb  1.30 2.85e-02           DISS
    ## 138          Qdpr  1.50 2.85e-02           DISS
    ## 139       Slc12a2  2.10 2.85e-02           DISS
    ## 140       Tmem170  2.50 2.93e-02           DISS
    ## 141          Apod  2.00 3.00e-02           DISS
    ## 142         Cadm2 -1.20 3.00e-02           HOMO
    ## 143         Erdr1  1.30 3.00e-02           DISS
    ## 144          Il1b  2.40 3.00e-02           DISS
    ## 145       Plekhg3  2.60 3.00e-02           DISS
    ## 146          Ccl4  1.90 3.00e-02           DISS
    ## 147         Sept4  1.80 3.00e-02           DISS
    ## 148          Cd82  2.20 3.21e-02           DISS
    ## 149         Cspg5  1.30 3.21e-02           DISS
    ## 150         Magi2 -1.30 3.21e-02           HOMO
    ## 151          Ucp2  2.50 3.21e-02           DISS
    ## 152         Smoc2  3.80 3.27e-02           DISS
    ## 153        mt-Nd3  1.70 3.28e-02           DISS
    ## 154          Fosb  1.60 3.32e-02           DISS
    ## 155          Mfng  4.00 3.32e-02           DISS
    ## 156       Pcdhga8  1.40 3.32e-02           DISS
    ## 157          Emp2  2.30 3.33e-02           DISS
    ## 158          Cd37  4.10 3.36e-02           DISS
    ## 159         Scrg1  2.60 3.36e-02           DISS
    ## 160           Maz  0.89 3.46e-02           DISS
    ## 161         Prr18  2.30 3.57e-02           DISS
    ## 162        mt-Nd1  1.20 3.60e-02           DISS
    ## 163         Actr2 -0.87 3.76e-02           HOMO
    ## 164       Rasgrp1 -1.10 3.78e-02           HOMO
    ## 165         Rock2 -1.00 3.88e-02           HOMO
    ## 166         Asap1 -1.90 4.01e-02           HOMO
    ## 167         Cd274  4.20 4.12e-02           DISS
    ## 168         Rpl32  1.30 4.12e-02           DISS
    ## 169         Synpr  2.30 4.12e-02           DISS
    ## 170      Cacna2d1 -1.20 4.17e-02           HOMO
    ## 171         Pold1  2.20 4.17e-02           DISS
    ## 172          Rpl5  0.85 4.17e-02           DISS
    ## 173          Btg2  1.40 4.21e-02           DISS
    ## 174       Gm42878 -3.80 4.36e-02           HOMO
    ## 175         Kcnj6 -2.40 4.36e-02           HOMO
    ## 176         Crtc2  1.80 4.37e-02           DISS
    ## 177           Msn  1.70 4.55e-02           DISS
    ## 178        mt-Nd4  1.20 4.55e-02           DISS
    ## 179          Rtn2  1.50 4.55e-02           DISS
    ## 180           Myc  2.30 4.56e-02           DISS
    ## 181         Bcas1  1.70 4.70e-02           DISS
    ## 182         Adcy9 -1.40 4.74e-02           HOMO
    ## 183          Gatm  2.10 4.74e-02           DISS
    ## 184          Gjb1  4.80 4.74e-02           DISS
    ## 185         Pros1  2.30 4.74e-02           DISS
    ## 186         Psat1  1.30 4.74e-02           DISS
    ## 187       Rab3il1  2.80 4.78e-02           DISS
    ## 188        Ppp3r1 -0.68 4.83e-02           HOMO
    ## 189         Arl4c  1.80 4.89e-02           DISS
    ## 190         Dhrs3  2.90 4.89e-02           DISS
    ## 191         Enpp2  1.80 4.89e-02           DISS
    ## 192         Gpr34  2.10 4.89e-02           DISS
    ## 193         Ntng1  3.10 4.89e-02           DISS
    ## 194        P2ry12  1.70 4.89e-02           DISS
    ## 195          Tlr2  2.60 4.89e-02           DISS
    ## 196       Cyp27a1  3.60 4.92e-02           DISS
    ## 197         Gna12  1.30 4.92e-02           DISS
    ## 198         Asap3  3.90 4.96e-02           DISS
    ## 199         Itgam  1.70 4.96e-02           DISS
    ## 200          Lcp1  2.70 4.96e-02           DISS
    ## 201         Frmd8  2.80 5.03e-02           DISS
    ## 202         Pink1  0.62 5.03e-02           DISS
    ## 203          Ncf1  1.80 5.07e-02           DISS
    ## 204        Dnajb2  1.50 5.14e-02           DISS
    ## 205       mt-Atp6  1.20 5.14e-02           DISS
    ## 206        mt-Co2  1.40 5.15e-02           DISS
    ## 207         Wasf2  1.70 5.20e-02           DISS
    ## 208         Ccdc3  5.40 5.32e-02           DISS
    ## 209          Aco2  0.66 5.37e-02           DISS
    ## 210          Fat3 -2.20 5.40e-02           HOMO
    ## 211          Smox  1.60 5.40e-02           DISS
    ## 212         Taok1 -1.30 5.40e-02           HOMO
    ## 213       Tmem63a  2.50 5.40e-02           DISS
    ## 214          Tpt1  0.96 5.40e-02           DISS
    ## 215          Vsir  2.10 5.40e-02           DISS
    ## 216         Gpr17  2.60 5.45e-02           DISS
    ## 217        Adgrl3 -1.40 5.70e-02           HOMO
    ## 218         Cmtm6  1.70 5.76e-02           DISS
    ## 219         Dock8  3.10 5.76e-02           DISS
    ## 220         Gp1bb  3.10 5.76e-02           DISS
    ## 221          Ier2  1.30 5.76e-02           DISS
    ## 222          Il16  3.30 5.76e-02           DISS
    ## 223         Rftn1  3.60 5.76e-02           DISS
    ## 224         Sox10  2.20 5.76e-02           DISS
    ## 225          Junb  1.00 5.81e-02           DISS
    ## 226        Pea15a  0.98 5.87e-02           DISS
    ## 227 D630003M21Rik  3.10 5.89e-02           DISS
    ## 228         Kcnk6  2.80 6.11e-02           DISS
    ## 229        Slc4a8 -1.00 6.11e-02           HOMO
    ## 230          Rela  1.50 6.18e-02           DISS
    ## 231       mt-Nd4l  1.10 6.21e-02           DISS
    ## 232         Itgb4  2.90 6.30e-02           DISS
    ## 233         Rpl11  0.96 6.30e-02           DISS
    ## 234        Zfp521  2.70 6.30e-02           DISS
    ## 235         Ilkap  1.60 6.32e-02           DISS
    ## 236       Nckap1l  2.10 6.32e-02           DISS
    ## 237         Ccrl2  3.00 6.48e-02           DISS
    ## 238          Gjc3  1.90 6.48e-02           DISS
    ## 239        Csrnp1  1.50 6.63e-02           DISS
    ## 240          Klf4  1.70 6.63e-02           DISS
    ## 241       Tbc1d16  1.50 6.66e-02           DISS
    ## 242         Wscd1  1.50 6.68e-02           DISS
    ## 243         Atxn1 -1.50 6.74e-02           HOMO
    ## 244       Arfgef3 -1.50 6.83e-02           HOMO
    ## 245      Arhgap32 -1.30 6.86e-02           HOMO
    ## 246          Atf5  1.60 6.86e-02           DISS
    ## 247        Dusp10  2.50 6.86e-02           DISS
    ## 248         Prdm5  3.60 6.86e-02           DISS
    ## 249        Arpc1b  2.00 6.89e-02           DISS
    ## 250        mt-Nd2  1.20 6.94e-02           DISS
    ## 251         Pde4d -1.80 6.94e-02           HOMO
    ## 252          Grap  3.00 7.08e-02           DISS
    ## 253         Rpl18  1.00 7.08e-02           DISS
    ## 254      Suv420h2  2.20 7.08e-02           DISS
    ## 255          Taf1 -1.20 7.15e-02           HOMO
    ## 256         Il6ra  2.20 7.21e-02           DISS
    ## 257          Npr1  3.30 7.25e-02           DISS
    ## 258         Clock -1.20 7.25e-02           HOMO
    ## 259          Glg1 -0.99 7.25e-02           HOMO
    ## 260        Smtnl2  5.80 7.25e-02           DISS
    ## 261       mt-Atp8  1.10 7.37e-02           DISS
    ## 262        Piezo1  3.10 7.37e-02           DISS
    ## 263          Pim1  2.40 7.37e-02           DISS
    ## 264        Rpl23a  1.00 7.37e-02           DISS
    ## 265         Rpl31  1.10 7.37e-02           DISS
    ## 266         Rplp1  1.10 7.40e-02           DISS
    ## 267        Atp2b1 -1.20 7.40e-02           HOMO
    ## 268          Cd63  1.60 7.40e-02           DISS
    ## 269          Csf1  1.90 7.40e-02           DISS
    ## 270         Prr36 -1.80 7.40e-02           HOMO
    ## 271         Myo1f  4.80 7.47e-02           DISS
    ## 272        Insig1  1.60 7.50e-02           DISS
    ## 273         Itpk1  1.50 7.58e-02           DISS
    ## 274        Lhfpl2  2.20 7.69e-02           DISS
    ## 275         Chadl  1.80 7.69e-02           DISS
    ## 276         Nlrp3  2.20 7.69e-02           DISS
    ## 277        Sh3gl1  1.60 7.69e-02           DISS
    ## 278         Cidea  5.50 7.85e-02           DISS
    ## 279         Rack1  1.00 7.85e-02           DISS
    ## 280        Sema4g  2.30 7.95e-02           DISS
    ## 281         Rpl23  1.20 8.02e-02           DISS
    ## 282        Adam17  2.30 8.05e-02           DISS
    ## 283       Cacna1e -1.30 8.05e-02           HOMO
    ## 284           Cpm  2.90 8.15e-02           DISS
    ## 285        Cox4i1  1.00 8.27e-02           DISS
    ## 286       Nectin1  1.80 8.27e-02           DISS
    ## 287         Ostf1  2.40 8.27e-02           DISS
    ## 288          Gnaq -1.20 8.28e-02           HOMO
    ## 289         Maml2  2.20 8.28e-02           DISS
    ## 290         Syt14 -1.50 8.36e-02           HOMO
    ## 291         Ap1s1  1.10 8.38e-02           DISS
    ## 292       Fam124a  2.40 8.38e-02           DISS
    ## 293            Hr  2.50 8.38e-02           DISS
    ## 294        Nfatc1  3.60 8.38e-02           DISS
    ## 295       Rps6kb2  1.70 8.38e-02           DISS
    ## 296        Stk32b  5.40 8.38e-02           DISS
    ## 297         Scn8a -0.93 8.52e-02           HOMO
    ## 298         Unc5b  2.00 8.52e-02           DISS
    ## 299        Cdkn1a  2.50 8.54e-02           DISS
    ## 300       Fam102b  1.50 8.75e-02           DISS
    ## 301         Garem -1.40 8.88e-02           HOMO
    ## 302         Ppm1e -0.85 9.01e-02           HOMO
    ## 303         Pde4c -4.00 9.02e-02           HOMO
    ## 304        Sh3d19  2.40 9.07e-02           DISS
    ## 305        Tspan2  1.90 9.07e-02           DISS
    ## 306       Tnfaip6  2.80 9.09e-02           DISS
    ## 307          Cd68  2.30 9.12e-02           DISS
    ## 308         Lamp1  0.87 9.12e-02           DISS
    ## 309         Mmp15  1.90 9.20e-02           DISS
    ## 310         Lamb2  2.70 9.22e-02           DISS
    ## 311         Wasf1 -0.68 9.22e-02           HOMO
    ## 312         Itpkb  1.50 9.25e-02           DISS
    ## 313        Il10ra  2.50 9.28e-02           DISS
    ## 314        Rps27a  1.10 9.33e-02           DISS
    ## 315        Rpl35a  1.50 9.44e-02           DISS
    ## 316          Tbx1  5.70 9.56e-02           DISS
    ## 317          Srgn  2.40 9.58e-02           DISS
    ## 318          Apoe  1.30 9.59e-02           DISS
    ## 319        Atp2c1 -1.10 9.59e-02           HOMO
    ## 320          Bche  3.30 9.59e-02           DISS
    ## 321        Cacnb2 -1.20 9.59e-02           HOMO
    ## 322          Ccl9  3.30 9.59e-02           DISS
    ## 323          Cd81  1.00 9.59e-02           DISS
    ## 324           Cfh  2.00 9.59e-02           DISS
    ## 325          Ctsc  2.40 9.59e-02           DISS
    ## 326        Dusp26  2.00 9.59e-02           DISS
    ## 327         Eif3h  0.84 9.59e-02           DISS
    ## 328         Erbin  1.70 9.59e-02           DISS
    ## 329          Gjc2  2.40 9.59e-02           DISS
    ## 330         Lrtm2  1.80 9.59e-02           DISS
    ## 331         Plin4  4.10 9.59e-02           DISS
    ## 332         Rgs10  2.20 9.59e-02           DISS
    ## 333      Rnaset2b  2.00 9.59e-02           DISS
    ## 334        Rpl18a  0.95 9.59e-02           DISS
    ## 335          Rpsa  1.10 9.59e-02           DISS
    ## 336      Slc25a10  2.90 9.59e-02           DISS
    ## 337         Spry1  2.20 9.59e-02           DISS
    ## 338        Tango2  1.80 9.59e-02           DISS
    ## 339        Ubqln1 -1.10 9.74e-02           HOMO
    ## 340       Gadd45b  1.50 9.84e-02           DISS
    ## 341         Gsk3b -0.90 9.84e-02           HOMO
    ## 342          Atrx -1.10 9.92e-02           HOMO
    ## 343         Itpr3  3.10 9.92e-02           DISS
    ## 344         Gria2 -0.84 9.97e-02           HOMO

    write.csv(DEGes, file = "../results/SuppTable1.csv", row.names = F)

Compare to cannonical learning and memory genes
-----------------------------------------------

    candidates <- dissociation %>%
      dplyr::filter(grepl('Ncs|Nsf|Gria|Grin|Grim|Dlg|Prkc|Camk2|Fmr1|Creb', gene)) %>%
      droplevels()
    candidates <- candidates[,c(1)]
    candidates

    ##  [1] Camk2a  Camk2b  Camk2d  Camk2g  Camk2n1 Camk2n2 Creb1   Creb3  
    ##  [9] Creb3l1 Creb3l2 Crebbp  Crebl2  Crebrf  Crebzf  Dlg1    Dlg2   
    ## [17] Dlg3    Dlg4    Dlg5    Dlgap1  Dlgap2  Dlgap3  Dlgap4  Fmr1   
    ## [25] Gria1   Gria2   Gria3   Gria4   Grin1   Grin2a  Grin2b  Grin2c 
    ## [33] Grin2d  Grin3a  Grina   Ncs1    Ncstn   Nsf     Nsfl1c  Prkca  
    ## [41] Prkcb   Prkcd   Prkcdbp Prkce   Prkcg   Prkci   Prkcq   Prkcsh 
    ## [49] Prkcz  
    ## 49 Levels: Camk2a Camk2b Camk2d Camk2g Camk2n1 Camk2n2 Creb1 ... Prkcz

Compare to cannonical learning and memory genes
-----------------------------------------------

    overlap <- DEGes %>%
      dplyr::filter(grepl('Ncs|Nsf|Gria|Grin|Grim|Dlg|Prkc|Camk2|Fmr1|Creb', gene)) %>%
      droplevels()
    overlap

    ##     gene   lfc   padj upregulated in
    ## 1 Grin2b -1.70 0.0132           HOMO
    ## 2 Grin2a -1.70 0.0269           HOMO
    ## 3  Gria2 -0.84 0.0997           HOMO