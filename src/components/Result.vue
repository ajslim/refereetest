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

        <h6 class="text-muted">Votes so far ({{ action.votes.total }})</h6>
        <div class="row">
            <div class="col">
                <b>Left</b>
                {% for call, vote in action.votes.left %}
                <div>
                    {{ call|capitalize }} : {{ vote }}
                </div>
                {% if action.votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (vote / action.votes.total) * 100 }}%;">{{ ((vote / action.votes.total) * 100)|round }}%</div>
                {% endif %}
                {% endfor %}
            </div>
            <div class="col">
                <b>Right</b>
                {% for call, vote in action.votes.right %}


                <div>
                    {{ call|capitalize }} : {{ vote }}
                </div>
                {% if action.votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (vote / action.votes.total) * 100 }}%;">{{ ((vote / action.votes.total) * 100)|round }}%</div>
                {% endif %}
                {% endfor %}
            </div>
        </div>

        <div class="row">
            <div class="col">
                <b>Neither</b>

                <div>
                    Simultaneous : {{ action.votes.simultaneous }}
                </div>

                {% if action.votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (action.votes.simultaneous / action.votes.total) * 100 }}%;">{{ ((action.votes.simultaneous  / action.votes.total) * 100)|round }}%</div>
                {% endif %}

            </div>
            <div class="col">
            </div>
        </div>

        <hr>

        <h6 class="text-muted">Cards</h6>
        <div class="row">
            <div class="col">
                Card Left: {{ action.votes.cardLeft }}
                {% if action.votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (action.votes.cardLeft / action.votes.total) * 100 }}%;">{{ ((action.votes.cardLeft / action.votes.total) * 100)|round }}%</div>
                {% endif %}
            </div>
            <div class="col">
                Card Right: {{ action.votes.cardRight }}
                {% if action.votes.total > 0 %}
                <div class="graph-bar" style="width: {{ (action.votes.cardRight / action.votes.total) * 100 }}%;">{{ ((action.votes.cardRight / action.votes.total) * 100)|round }}%</div>
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
