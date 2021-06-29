# cflow

cflowは、C言語のフローグラフを生成する。

実際に例を見たほうがわかりやすいのでGNu cflowのサンプルを一部引用してみる。

<https://www.gnu.org/software/cflow/sample.html>

~~~
1 main() <int main (int argc,char **argv) at main.c:627>:
2     register_output() <int register_output (const char *name,int (*handler)(cflow_output_command cmd,FILE *outfile,int line,void *data,void *handler_data),void *handler_data) at output.c:74>:
3         abort()
4         strdup()
5         handler()
6     gnu_output_handler() <int gnu_output_handler (cflow_output_command cmd,FILE *outfile,int line,void *data,void *handler_data) at gnu.c:63>:
7         fprintf()
8     posix_output_handler() <int posix_output_handler (cflow_output_command cmd,FILE *outfile,int line,void *data,void *handler_data) at posix.c:74>:
9         fprintf()
10     xstrdup()
~~~

main関数はまずregister_output関数を呼び、その関数の中ではabort, stardup, handlerが呼ばれ、といった具合に、関数呼び出しに着目した呼び出し元、呼び出し先の一覧を生成してくれる。

どのくらい役に立つかはよくわからない。関数は呼び出す可能性があるというだけだ。たとえばabortが呼び出されるのは条件次第だ。実際にはregister_outputは以下のようになっているわけだが、

~~~c
int
register_output(const char *name,
		int (*handler) (cflow_output_command cmd,
				FILE *outfile, int line,
				void *data, void *handler_data),
		void *handler_data)
{
     if (driver_max == MAX_OUTPUT_DRIVERS-1)
	  abort ();
     output_driver[driver_max].name = strdup(name);
     output_driver[driver_max].handler = handler;
     output_driver[driver_max].handler_data = handler_data;
     return driver_max++;
}
~~~

<https://git.savannah.gnu.org/cgit/cflow.git/tree/src/output.c#n72>

cflowは関数のシグネチャと呼び出し元呼び出し先の関係をパースすることにしか関心がない。C言語をパースするコードは以下にある。

<https://git.savannah.gnu.org/cgit/cflow.git/tree/src/parser.c>



HaskellやErlangのような関数という単位がとても強い意味を持つ言語ですら、こういうツールがあったところであまりありがたみはないだろうと思われる。ところでcflowはC言語限定だ。

POSIXには本当に規格化する価値のわからないユーティリティが多い。

<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/cflow.html>
