
## Exercices :

1. What is the ip address of adlp-corp.com ?
    
    > 52.51.133.160 
    > 
    > `dig adlp-corp.com +short
    
2. What is the TXT record of adlp-corp.com?
    
    > "BC{DESCRIPTIVE-DOMAIN-TXT}"  
    > `dig -t txt adlp-corp.com +short
    
3. What are the MX records of becode.org ?
    
    > 10 alt3.aspmx.l.google.com.
    > 5 alt2.aspmx.l.google.com.
    > 1 aspmx.l.google.com.
    > 5 alt1.aspmx.l.google.com.
    > 10 alt4.aspmx.l.google.com.
    > 
    > `dig -t mx becode.org +short
    
4. What are the MX records of adlp-corp.com ?
    
    > 10 alt3.aspmx.l.google.com.
	   10 alt4.aspmx.l.google.com.
	   1 aspmx.l.google.com.
	   5 alt2.aspmx.l.google.com.
	   5 alt1.aspmx.l.google.com.
	   
	   `dig -t mx adlp-corp.com +short`
    
5. What is the first NS name server of adlp-corp.com?
    
    > ns-269.awsdns-33.com.
    > 
    > `dig -t ns adlp-corp.com +short`
    
6. Uses a brute force tool to find subdomains of adlp-corp.com. How many did you find?
    
    >Found: admin.adlp-corp.com

	Found: ftp.adlp-corp.com

	Found: job.adlp-corp.com

	Found: mail2.adlp-corp.com

	Found: mail.adlp-corp.com
	
	Found: smtp.adlp-corp.com
    > 
    
7. Use theHarvester tool at becode.org. How many Linkedin Users?
    
    > Your response Your command
    
8. Use theHarvester tool at becode.org. How many ip addresses did you find?
    
    > badge.becode.org: 63.35.76.110
	   dokiman.becode.org: 52.208.172.244
       planning.becode.org: 34.242.151.5
       proxy.becode.org: 52.208.172.244
       staging.becode.org: 34.245.125.111
       vmsdt-mendes.becode.org: 84.199.185.118
    
9. Write a small script to attempt a zone transfer from adlp-corp.com using a higher-level scripting language such as Python, Perl, or Ruby

import dns.query
import dns.zone
import dns.resolver
import sys

def attempt_zone_transfer(domain):
    try:
        ns_records = dns.resolver.resolve(domain, 'NS')
        for ns in ns_records:
            try:
                zone = dns.zone.from_xfr(dns.query.xfr(ns.to_text(), domain))
                print(f"Transfert de zone réussi depuis {ns.to_text()}")
                for name, node in zone.nodes.items():
                    for rdataset in node.rdatasets:
                        print(name, rdataset)
                return
            except Exception as e:
                print(f"Échec du transfert de zone depuis {ns.to_text()} : {e}")
        print("Échec du transfert de zone depuis tous les serveurs de noms")
    except Exception as e:
        print(f"Échec de la résolution des enregistrements NS pour {domain} : {e}")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python zone_transfer.py <nom_de_domaine>")
        sys.exit(1)
    attempt_zone_transfer(sys.argv[1])
`
    
10. Write a small script to attempt a brute force search for subdomains using a higher level scripting language such as Python, Perl or Ruby.
    
    > import dns.resolver
	   import sys

		def brute_force_subdomains(domain, wordlist_file):
		    try:
		        with open(wordlist_file, 'r') as file:
		            subdomains = [line.strip() + '.' + domain for line in file]

		        for subdomain in subdomains:
		            try:
		                answers = dns.resolver.resolve(subdomain, 'A')
		                print(f"{subdomain} - {answers[0]}")
		            except (dns.resolver.NXDOMAIN, dns.resolver.NoAnswer):
		                pass
		            except Exception as e:
		                print(f"Erreur en résolvant {subdomain}: {e}")

				    except FileNotFoundError:
				        print(f"Fichier non trouvé: {wordlist_file}")

		if __name__ == "__main__":
		    if len(sys.argv) != 3:
		        print("Usage: python subdomain_bruteforce.py <domain> <wordlist_file>")
        sys.exit(1)
		    brute_force_subdomains(sys.argv[1], sys.argv[2])
