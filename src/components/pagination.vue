<template>
  <nav>
    <span class="pagination page-link m-r-1">total:{{total}}</span>
    <ul class="pagination">
      <li @click="setPage(unit.page)" track-by="$index" v-for="unit in units" class="page-item {{unit.class}}"
          :disabled="unit.disabled">
        <a class="page-link" aria-label="{{unit.ariaLabel}}">
          <span v-if="unit.isPager" aria-hidden="true">{{{unit.html}}}</span>
          <span v-else>{{{unit.html}}}</span>
          <span v-if="unit.isPager" class="sr-only">{{{unit.srHtml}}}</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script>
  export default {
    data(){
      return {
        prevHtml: '«',
        nextHtml: '»',
        prevSrHtml: 'Previous',
        nextSrHtml: 'Next',
        dotsHtml: '...'
      }
    },
    props: {
      page: {
        type: Number,
        default: 1
      },
      total: {
        type: Number,
        required: true
      },
      pageSize: {
        type: Number,
        default: 10
      },
      maxLink: {
        type: Number,
        default: 5
      },
      eventName: {
        type: String,
        required: true
      }
    },
    methods: {
      setPage(page){
        if (page === this.page) return false
        this.page = page
        this.$dispatch(this.eventName, page)
      }
    },
    computed: {
      units(){
        var th = this
        var page = th.page
        var pageSize = th.pageSize
        var total = th.total
        var maxLink = th.maxLink > 5 ? th.maxLink : 5

        var linksCount = Math.ceil(total / pageSize)

        if (page > linksCount) page = linksCount + 0

        var hasPrev = page > 1
        var hasNext = page < linksCount
        var realMaxLink = maxLink > linksCount ? linksCount : maxLink

        var units = []
        var arr = computeLens()

        units.push({
          class: hasPrev ? '' : 'disabled',
          page: hasPrev ? page - 1 : page,
          isPager: true,
          isPrev: true,
          isNext: false,
          html: th.prevHtml,
          srHtml: th.prevSrHtml,
          ariaLabel: th.prevSrHtml
        })

        var dotUnit = {
          class: 'disabled' ,
          page: page,
          isPager: false,
          isPrev: false,
          isNext: true,
          html: th.dotsHtml
        }

        for (var i = 0, len = arr.length; i < len; i++) {
          pushUnit(arr[i])
        }

        units.push({
          class: hasNext ? '' : 'disabled',
          page: hasNext ? page + 1 : page,
          isPager: true,
          isPrev: false,
          isNext: true,
          html: th.nextHtml,
          srHtml: th.nextSrHtml,
          ariaLabel: th.nextSrHtml
        })

        function pushUnit(i) {
          if (typeof i === 'number') {
            units.push({
              page: i
              , isPrev: false
              , isPager: false
              , disabled: false
              , class: i === page ? 'active' : ''
              , isNext: false
              , html: i
            })
          } else units.push(dotUnit)
        }

        function computeLens() {
          var a4 = Math.floor((realMaxLink - 2) / 2)
          var a5 = realMaxLink - 3 - a4
          var s2 = page - a4
          var s3 = page + a5
          if (s2 < 2) {
            s2 = 2
          }
          else if (s3 > linksCount) {
            s2 = linksCount - (realMaxLink - 2)
          }
          var arr = [1]
          if (s2 > 2) arr.push('dot')
          var it
          for (var i = 0, len = realMaxLink - 2 < 1 ? realMaxLink - 1 : realMaxLink - 2; i < len; i++) {
            it = i + s2
            arr.push(it)
          }
          if (it < linksCount - 1) arr.push('dot')
          if (it < linksCount) arr.push(linksCount)
          return arr
        }

        return units
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .m-r-1 {
    margin-right: 1rem!important;
  }
  .page-link {
    position: relative;
    float: left;
    padding: .5rem .75rem;
    margin-left: -1px;
    line-height: 1.5;
    text-decoration: none;
    background-color: #fff;
    border: 1px solid #ddd;
  }
</style>
