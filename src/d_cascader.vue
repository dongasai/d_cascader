<style scoped>
    select{
        background-color: white;
        width: 145px;
        height: 36px;
        margin-right: 10px;
        line-height: 28px;
        font-size: 16px;
        float: left;
    }
</style>
<template>
    <div>
1414
<!-- d_cascader -->	
        <select v-model="now_value" >
            <option
                    v-for="info in  cascade_data"
                    v-bind:value="info[aiindex]">
                {{ info[nameindex] }}
            </option>
        </select>

        <d_cascader v-if="children_bl"
                    :cdata="children"
                    @cascader_change="Cascader_change"
                    @push_change="push_change"
                    :max_level="max_level"
                    :de_value="s_de_value"
                    :level="level+1"
                    :upindex="upindex"
                    :d_type="'g'">
        </d_cascader>
    </div>
</template>
<script>

    export default {
        name: 'd_cascader',
        props: {
            cdata: {
                type: [Array, Object],
                default(){
                    let dd=[{
                        id: "1",
                        name: "默认项",
                        upid: "0",
                    }];;
                   return  dd;
                }
            },
            upindex: {
                type: String,
                default() {
                    return 'upid';
                }
            },
            d_type: {
                type: String,
                default() {
                    return 'list';
                }
            },
            aiindex: {
                type: String,
                default: 'id'
            },
            nameindex: {
                type: String,
                default: 'name'
            },
            level: {
                type: Number,
                default: 0,
            },
            max_level: {
                type: Number,
                default: 999
            },
            de_value: {
                type: Array,
                default() {
                    return [];
                }
            }
        },
        data: function () {
            return {
                value_set:false,
                s_de_value:[],
                emit_value: null,
                now_value: null,
                children: {},
                children_bl: !1
            };
        },
        components: {},
        computed: {
            list_data: function () {
                return 'list' == this.d_type ? this.cdata : this.to_list(this.cdata);
            },
            cascade_data () {
                if ('list' == this.d_type){
                    var t = this.tograding(this.cdata);
                }else{
                    var t = this.cdata;
                }
                console.log(t);
                if(this.now_value==null || !(this.now_value in t)){
                    this.now_value=t[Object.keys(t)[0]][this.aiindex];
                }

                return t;
            }
        },
        watch: {
            now_value(t, n) {
                if(t!= n){
                    console.log('now_value-watch:',this.level,t);
                    this.to_children(t);
                }
            },
            de_value(new_value){
                console.error('de_value:',this.level,new_value);
                this.de_value_set();
            }
        },
        methods: {
            change_a: function (t) {
                this.to_children(this.now_value);
            },
            to_children: function (value) {
                if ('' == value){
                    return !1;
                }
                var n = this.cascade_data[value];
                if(typeof n != 'object'){
                    console.warn('error-value:',value,this.cascade_data)
                    return ;
                }
                this.emit(this.level, this.now_value, n);
                this.now_value = value;

                if ('object' == typeof (n.children) && this.level < this.max_level - 1) {
                    this.children = n.children;
                    this.children_bl = true;
                }
            },
            emit: function (t, n, e) {
                console.log('emit',t,n,e,this.level,this.emit_value);
                if(n != this.emit_value ) {
                    this.$emit('cascader_change', {
                        lv: t,
                        value: n,
                        data: e
                    });
                    if(t == this.level){
                        this.emit_value = n;
                    }
                }

            },
            tograding: function (t) {
                return this.grading(t, this.upindex, this.aiindex, 0, 0, 0)
            },
            grading: function (t, n, e, r, o) {
                var i = {}, u = 0;
                for (var c in t) {
                    var a = t[c];
                    void 0 === i[a[n]] && (u++, i[a[n]] = []), i[a[n]].push(a)
                }
                return this.grading2(i, n, e, r, o)
            },
            grading2: function (t, n, e, r, o) {
                var i = {}, u = t[r];
                if (void 0 === r)return !1;
                if (void 0 === u)return !1;
                for (var c in u) {
                    var a = u[c];
                    i[a[e]] = a;
                    var f = this.grading2(t, n, e, a[e], o + 1);
                    0 != f && (i[a[e]].children = f), i[a[e]].level = o;
                }
                return i;
            },
            Cascader_change: function (t) {

                if(0 == this.level){
                    this.push_change(t);
                } else{
                    this.$emit('push_change', t);
                }
            },
            push_change: function (t) {
                this.emit(t.lv, t.value, t.data);
            },
            de_value_set(){
                if(this.value_set==false){
                    if(this.de_value.length > 0){
                        this.value_set=true;
                        let dd_value =this.de_value.shift();
                        if(dd_value in this.cascade_data){
                            this.now_value=dd_value;
                        }
                        this.s_de_value=this.de_value;
                        console.warn('mounted:',this.level,this.de_value,dd_value);
                    }

                }

            }
        },
        created: function () {
            let index=this.cascade_data[Object.keys(this.cascade_data)[0]][this.aiindex];
            console.warn('created:',this.level,index);
            this.to_children(index);
        },
        mounted: function () {
            //chuangji
            this.de_value_set();
        },
        updated: function () {
        }

    };
</script>

