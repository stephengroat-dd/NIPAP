START=^

clean:
	for I in `seq 32|sort -nr`; do echo "Deleting IPv4 prefixes with length: $$I"; psql -c "DELETE FROM ip_net_plan WHERE family(prefix) = 4 AND masklen(prefix)=$$I" nap > /dev/null; done ;

transfer:
	rm -f 1 2
	scp $(TEXTFILE) 1
	chmod u+w 1

utf8:
	iconv --from iso-8859-1 --to utf-8 1 > 2
	mv 2 1

except:
	grep -v -f exceptions 1 > 2
	mv 2 1

import:
	./text-import.py --schema global --url http://dev@local:dev@127.0.0.1:1337 1

genpartial:
	sed -ne '/$(START)/,$$p' 1 > 2
	mv 2 1

reread: clean except genpartial import

full:	clean transfer utf8 except genpartial import

partial:	clean transfer utf8 except genpartial import
