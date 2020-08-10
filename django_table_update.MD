## insert the following in html file
'''
html
    head
        <script>
            <!-- update "post_table1" with your url name which is mentioned in urls.py, eg:- "url(r'table1/$', csrf_exempt(pv.post_table), name="post_table1"),," -->
            var URL = "{% url 'post_table1' %}"
        </script>
    head
    
    
    <script>
<!--# Function to Retrive data from table and post to view-->
function update(){
        var list =  ""
        var arr = []
<!--# Upadate the table name-->
        $( function fn() { $('.table tr').each(
        function firstFunction(rowindex) {
            var line = ""
            $.each(this.cells, function (cellIndex, cell){
                  line+=cell.textContent+',';
                    });
                list = line
                arr.push(list)
            });
            var i;
            for (i=1; i<arr.length; i++){
            var data = { "data": arr[i] }
            $.post(URL, data, function(response){ } );
            }
    });

}

<!--# OnClick Button Function to execute update function-->
$(document).ready(function(){
  $('#update').click(function (){

            l = update()
     });
   });

</script>

...


## IN Views.PY


'''
def post_table(request):
# check if post is sussesfull and store the value in alldata variable
    if request.method == 'POST':
        alldata = request.POST
# converting QueryDict dictionery and pushing value to models
        for k,v in alldata.items():
# converting string to list
            vl=v.split(",")
# removing last item in list
            vl.pop()
# updaating value to model
            a = posts(pk = vl[0],title = vl[1],description = vl[2],timestamp="2020-10-10")
# save
            a.save()
    qs = posts.objects.all().order_by("-timestamp")
    context = {
        "qs": qs,
    }
    return render(request,"post_tb_update.html", context)

'''
