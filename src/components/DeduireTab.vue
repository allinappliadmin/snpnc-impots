<template>
  <b-card no-body>
    <b-tabs card fill>

      <b-tab title="Idemnités Journalières" active lazy><b-card-text>
        <IdemnitesJournalieres :id="f.iJ.id" v-bind:edit="true" v-model="iJ" v-bind:field="f.iJ">
        </IdemnitesJournalieres>
      </b-card-text></b-tab>

      <b-tab title="Frais transport" lazy><b-card-text>

        <ValidationProvider :name="f.fTAp.label.toLowerCase()" rules="" v-slot="vCtx">
          <b-form-group :id="f.fTAp.id + '-group' " :label="f.fTAp.label" :label-for="f.fTAp.id">
            <b-form-text :id="f.fTAp.id + '-help'">{{ f.fTAp.description }}</b-form-text>
            <b-form-select
              :id="f.fTAp.id" v-model="fTAp" :state="getValidationState(vCtx)" :options="fTApoptions"/>
            <b-form-invalid-feedback :id="f.fTAp.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>
        <b-collapse :visible="fTAp !== f.fTAp.default" class="mt-2">
          <b-form-group :id="f.fTAd.id + '-group' " :label="f.fTAd.label" :label-for="f.fTAd.id">
            <MonthlyTable
              :id="f.fTAd.id"
              v-bind:edit="true"
              v-model="fTAd"
              v-on:input="updateTransportFTA()"
              v-bind:field="f.fTAd">
            </MonthlyTable>
          </b-form-group>
        </b-collapse>

        <ValidationProvider :name="f.fTMp.label.toLowerCase()" rules="" v-slot="vCtx">
          <b-form-group :id="f.fTMp.id + '-group' " :label="f.fTMp.label" :label-for="f.fTMp.id">
            <b-form-text :id="f.fTMp.id + '-help'">{{ f.fTMp.description }}</b-form-text>
            <b-form-select :id="f.fTMp.id" v-model="fTMp" :state="getValidationState(vCtx)" :options="fTMpoptions"/>
            <b-form-invalid-feedback :id="f.fTMp.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>
        <b-collapse :visible="fTMp !== f.fTMp.default" class="mt-2">
          <b-form-group :id="f.fTMd.id + '-group' " :label="f.fTMd.label" :label-for="f.fTMd.id">
            <MonthlyTable
              :id="f.fTMd.id"
              v-bind:edit="true"
              v-model="fTMd"
              v-on:input="updateTransportFTM()"
              v-bind:field="f.fTMd">
            </MonthlyTable>
          </b-form-group>
        </b-collapse>

        <b-form-group :id="f.fTC.id + '-group' " :label="f.fTC.label" :label-for="f.fTC.id">
          <MonthlyTable :id="f.fTC.id" v-bind:edit="true" v-model="fTC" v-bind:field="f.fTC"></MonthlyTable>
        </b-form-group>

      </b-card-text></b-tab>

      <b-tab title="Cotisations syndicales" lazy><b-card-text>

        <ValidationProvider :name="f.cS.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.cS.id + '-group' " :label="f.cS.label" :label-for="f.cS.id">
            <b-form-text :id="f.cS.id + '-help'">{{ f.cS.description }}</b-form-text>
            <b-form-input :id="f.cS.id" v-model="cS" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.cS.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

      </b-card-text></b-tab>

      <b-tab title="Autres frais" lazy><b-card-text>

        <ValidationProvider :name="f.fB.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fB.id + '-group' " :label="f.fB.label" :label-for="f.fB.id">
            <b-form-text :id="f.fB.id + '-help'">{{ f.fB.description }}</b-form-text>
            <b-form-input :id="f.fB.id" v-model="fB" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fB.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <ValidationProvider :name="f.fCMB.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fCMB.id + '-group' " :label="f.fCMB.label" :label-for="f.fCMB.id">
            <b-form-text :id="f.fCMB.id + '-help'">{{ f.fCMB.description }}</b-form-text>
            <b-form-input :id="f.fCMB.id" v-model="fCMB" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fCMB.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <ValidationProvider :name="f.fTI.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fTI.id + '-group' " :label="f.fTI.label" :label-for="f.fTI.id">
            <b-form-text :id="f.fTI.id + '-help'">{{ f.fTI.description }}</b-form-text>
            <b-form-input :id="f.fTI.id" v-model="fTI" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fTI.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <ValidationProvider :name="f.fBa.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fBa.id + '-group' " :label="f.fBa.label" :label-for="f.fBa.id">
            <b-form-text :id="f.fBa.id + '-help'">{{ f.fBa.description }}</b-form-text>
            <b-form-input :id="f.fBa.id" v-model="fBa" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fBa.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <ValidationProvider :name="f.fDR.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fDR.id + '-group' " :label="f.fDR.label" :label-for="f.fDR.id">
            <b-form-text :id="f.fDR.id + '-help'">{{ f.fDR.description }}</b-form-text>
            <b-form-input :id="f.fDR.id" v-model="fDR" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fDR.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <ValidationProvider :name="f.fL.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fL.id + '-group' " :label="f.fL.label" :label-for="f.fL.id">
            <b-form-text :id="f.fL.id + '-help'">{{ f.fL.description }}</b-form-text>
            <b-form-input :id="f.fL.id" v-model="fL" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fL.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <ValidationProvider :name="f.fF.label.toLowerCase()" rules="required|min:1|min_value:0" v-slot="vCtx">
          <b-form-group :id="f.fF.id + '-group' " :label="f.fF.label" :label-for="f.fF.id">
            <b-form-text :id="f.fF.id + '-help'">{{ f.fF.description }}</b-form-text>
            <b-form-input :id="f.fF.id" v-model="fF" :state="getValidationState(vCtx)"/>
            <b-form-invalid-feedback :id="f.fF.id + '-feedback'">{{ vCtx.errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

      </b-card-text></b-tab>

    </b-tabs>
  </b-card>
</template>

<script>

import fieldsMixin from '../model/fieldsMixin';
import modelFields from '../model/fields';
import MonthlyTable from './MonthlyTable';
import IdemnitesJournalieres from './IdemnitesJournalieres';

import modelData from '../model/data';

export default {
  mixins: [fieldsMixin],
  name: 'DeduireTab',
  components: {
    MonthlyTable,
    IdemnitesJournalieres
  },
  data() {
    // load auto and moto data.
    let fTApoptions = modelData.idtransport.auto.map(a => { return { value: a.key, text: a.label }} );
    let fTMpoptions = modelData.idtransport.moto.map(a => { return { value: a.key, text: a.label }} );

    return { f : modelFields, fTApoptions, fTMpoptions};
  },
  computed: {
    ...fieldsMixin.mapFields("deduire", "updateField", ["fTAp", "fTMp"]),
    ...fieldsMixin.mapFields("deduire", "updateIdemnitesJournalieres", ["iJ"]),
    ...fieldsMixin.mapFields("deduire", "updateDataTableField", ["fTAd", "fTMd", "fTC"]),
    ...fieldsMixin.mapFields(
      "deduire",
      "updateDataField",
      ["fTA", "fTM", "cS", "fB", "fCMB", "fTI", "fBa", "fDR", "fL", "fF"]
    ),
  },
  watch: {
    fTAp() {
      this.updateTransportFTA();
    },
    fTMp() {
      this.updateTransportFTM();
    },

  },
  methods: {
    updateTransportFTA() {
      if (this.fTAp === "null" || this.fTAd.tableTotal == 0) {
        modelFields.fTA.description = `Je n'ai pas de frais d'automobile à déduire`;
        this.$store.state['deduire']['fTA'] = 0;
      } else if (this.fTAd.tableTotal > 0)  {
        let bareme = modelData.idtransport.auto.find(e => e.key == this.fTAp);
        let distance = this.fTAd.tableTotal;
        bareme = bareme.distance.find(e => e.min < distance);

        modelFields.fTA.description = `
          Puissance ${this.fTAp}, distance ${distance} \n
          Total = ${distance} * ${bareme.coef} + ${bareme.ajust}
        `;
        let total = Math.round((distance * bareme.coef) + bareme.ajust);
        this.$store.state['deduire']['fTA'] = total;
      }
    },
    updateTransportFTM() {
      if (this.fTMp === "null" || this.fTAd.tableTotal == 0) {
        modelFields.fTM.description = `Je n'ai pas de frais d'automobile à déduire`;
        this.$store.state['deduire']['fTM'] = 0;
      } else if (this.fTMd.tableTotal > 0)  {
        let bareme = modelData.idtransport.moto.find(e => e.key == this.fTMp);
        let distance = this.fTMd.tableTotal;
        bareme = bareme.distance.find(e => e.min < distance);

        modelFields.fTM.description = `
          Puissance ${this.fTMp}, distance ${distance} \n
          Total = ${distance} * ${bareme.coef} + ${bareme.ajust}
        `;
        let total = Math.round((distance * bareme.coef) + bareme.ajust);
        this.$store.state['deduire']['fTM'] = total;
      }
    },
  },
}
</script>