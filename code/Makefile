makefiles/year_list.pkl: makefiles/make_year_list.py
	python makefiles/make_year_list.py

./data_setup/.psql_dts_sentinal: makefiles/year_list.pkl .psql_dts_sentinal
	python data_setup/create_postgres_dts.py makefiles/year_list.pkl
	touch ./data_setup/.psql_dts_sentinal

./data_setup/.merged_psql_sentinal: makefiles/year_list.pkl ./data_setup/.psql_dts_sentinal
	python data_setup/merge_shapefile_dts.py makefiles/year_list.pkl
	touch ./data_setup/.merged_psql_sentinal

./data_setup/.cleaned_psql_sentinal: makefiles/year_list.pkl ./data_setup/.merged_psql_sentinal
	python data_setup/clean_merged_dts.py makefiles/year_list.pkl
	touch ./data_setup/.cleaned_psql_sentinal

./data_setup/.csvs_jsons_sentinal: makefiles/year_list.pkl ./data_setup/.cleaned_psql_sentinal
	python data_setup/create_csvs_jsons.py makefiles/year_list.pkl
	aws s3 cp ../data/csvs/ s3://sallamander-forest-fires --recursive
	aws s3 cp ../data/jsons s3://sallamander-forest-fires --recursive
	touch ./data_setup/.csvs_jsons_sentinal

setup_data: ./data_setup/.psql_dts_sentinal ./data_setup/.merged_psql_sentinal \
			./data_setup/.cleaned_psql_sentinal ./data_setup/.csvs_jsons_sentinal
.PHONY: setup_data


	