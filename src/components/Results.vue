<template>
    <div class="col-lg-6">
        {% if verifiedString is defined %}
        <h6 class="text-muted">Verified</h6>
        <div class="row">
            <div class="col">
                This action has been verified as: {{ verifiedString }}
            </div>
        </div>
        <hr>
        {% endif %}

        <h6 class="text-muted">Votes so far ({{ votes.total }})</h6>
        <div class="row">
            <div class="col">
                <b>Left</b>
                {% for call, vote in votes.left %}
                <div>
                    {{ call|capitalize }} : {{ vote }}
                </div>
                {% if votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (vote / votes.total) * 100 }}%;">{{ ((vote / votes.total) * 100)|round }}%</div>
                {% endif %}
                {% endfor %}
            </div>
            <div class="col">
                <b>Right</b>
                {% for call, vote in votes.right %}


                <div>
                    {{ call|capitalize }} : {{ vote }}
                </div>
                {% if votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (vote / votes.total) * 100 }}%;">{{ ((vote / votes.total) * 100)|round }}%</div>
                {% endif %}
                {% endfor %}
            </div>
        </div>

        <div class="row">
            <div class="col">
                <b>Neither</b>

                <div>
                    Simultaneous : {{ votes.simultaneous }}
                </div>

                {% if votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (votes.simultaneous / votes.total) * 100 }}%;">{{ ((votes.simultaneous  / votes.total) * 100)|round }}%</div>
                {% endif %}

            </div>
            <div class="col">
            </div>
        </div>

        <hr>

        <h6 class="text-muted">Cards</h6>
        <div class="row">
            <div class="col">
                Card Left: {{ votes.cardLeft }}
                {% if votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (votes.cardLeft / votes.total) * 100 }}%;">{{ ((votes.cardLeft / votes.total) * 100)|round }}%</div>
                {% endif %}
            </div>
            <div class="col">
                Card Right: {{ votes.cardRight }}
                {% if votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (votes.cardRight / votes.total) * 100 }}%;">{{ ((votes.cardRight / votes.total) * 100)|round }}%</div>
                {% endif %}
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'app',
    components: {},
    props: {
      action: {
        type: Object,
        default: undefined,
      },
    },
    data: () => {
      return {
        playSlow: false
      }
    },
    methods: {
    },
  };
</script>

<style>
</style>
