				Copyright @ 2019 Grecu Andrei-George, All rights reserved
				
						Tema 3 - Exploit ELFs, not elves
				
1.
	Adresa functiei problema este 0x8048680, se apeleaza in <main+153> (mai exact 0x80487a4).
	Aceasta are o problema deoarece se produce un buffer overflow atunci cand se cere sa se citeasca (push 0x271)
		mai mult decat se poate (sub esp, 0x15e) (0x15e < 0x271).
	Pentru a descoperi asta am rula in gdb binarul si am dat next pana am intampinat aceasta problema.

2.
	Pentru a crea un payload suficient pentru a rescrie adresa lui eax (el fiind registrul care se
		apeleaza si se poate rescrie) am adunat toate push-urile (care reprezinta parametrul pentru functia de citire)
		din functiile anterioare celei cu apelul lui eax, si la suma am adunat ultimul sub din esp (sub esp, 0x15e).
	La suma am adunat 4 pentru old ebp, inca 4 pentru adresa de retur si ultimii 4 bytes sunt pentru adresa
		la care trebuie sarit (0x80485b1).
	Astfel s-a creat payload-ul pentru exercitiul 2, care are la final adresa unde trebuie sa sara
		programul pentru a afisa flagul:
				NICE_FLAG{a105a4e24fd8b89de86feb2c3527b560}

3.
	Am cautat ca la exercitiul 1 functia problema si am observat tot un buffer overflow la functia 0x80486a6.
	Dar pentru a ajunge acolo, mai intai trebuie sa cream payloadul pe parcurs, astfel incat sa intre pe acele jump-uri
		conditionate de cmp-uri.
	Se face payload-ul cala exercitiul 2, scriind caractere cate sunt aratate de push-ul corespunzator
		(parametrul functiei de citire).
	Stim ca trebuie pusa printre caracterele payload-ului nostru, o adresa cu care se compara ebp-urile, din care
		se scad o anumita valoare.
	Se creeaza pentru functia respectiva payload-ul in functie de cat este citit si apoi, de la un byte,
		aflat prin compararea adreselor lui ebp-x si adresele stack-ului, se scrie adresa care trebuie comparata,
		adresa de jump corecta (aici x e valoarea din cmp-urile fiecarei functii).
	Se repeta procesul pana se ajunge la functia cu "probleme", inclusiv, insa aici difera payload-ul, el fiind valoarea
		ce se scade din esp (0x191).
	La final am pus 4 pentru old ebp, inca 4 pentru adresa de retur si ultimii 4 bytes sunt pentru adresa
		la care trebuie sarit (0x80485b1).
	Astfel s-a creat payload-ul pentru exercitiul 3, care are la final adresa unde trebuie sa sara
		programul pentru a afisa flagul:
				NAUGHTY_FLAG{8c193feeca5ac9808a7cf32ce2cdf0e0}
